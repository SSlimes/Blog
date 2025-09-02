---
title: 【Hugo】hugo-stack主题魔改-归档下一页配置
date: 2025-09-02
categories:
  - 其他
image: images/28.webp
---
# 演示效果：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250902102246.png)
# 修改：
在`\layouts\_default`文件夹下创建：`archives.html`
```
{{ define "body-class" }}template-archives{{ end }}
{{ define "main" }}
    <header>
        {{- $taxonomy := $.Site.GetPage "taxonomyTerm" "categories" -}}
        {{- $terms := $taxonomy.Pages -}}
        {{ if $terms }}
        <h2 class="section-title">{{ $taxonomy.Title }}</h2>
        <div class="subsection-list">
            <div class="article-list--tile">
                {{ range $terms }}
                    {{ partial "article-list/tile" (dict "context" . "size" "250x150" "Type" "taxonomy") }}
                {{ end }}
            </div>
        </div>
        {{ end }}
    </header>

    {{ $pages := where .Site.RegularPages "Type" "in" .Site.Params.mainSections }}
    {{ $notHidden := where .Site.RegularPages "Params.hidden" "!=" true }}
    {{ $filtered := ($pages | intersect $notHidden) }}

    {{ range $filtered.GroupByDate "2006" }}
    {{ $id := lower (replace .Key " " "-") }}
    <div class="archives-group" id="{{ $id }}">
        <h2 class="archives-date section-title"><a href="{{ $.RelPermalink }}#{{ $id }}">{{ .Key }}</a></h2>
        <div class="article-list--compact" data-year="{{ .Key }}" data-total="{{ len .Pages }}">
            {{ range $index, $page := .Pages }}
                <div class="article-item" data-index="{{ $index }}" {{ if ge $index 8 }}style="display: none;"{{ end }}>
                    {{ partial "article-list/compact" $page }}
                </div>
            {{ end }}
        </div>
        
        {{/* 如果文章数量超过8篇，显示分页控件 */}}
        {{ if gt (len .Pages) 8 }}
            <div class="archives-pagination" data-year="{{ .Key }}">
                <button class="pagination-btn prev-btn" onclick="showPrevPage('{{ .Key }}')" disabled>上一页</button>
                <span class="pagination-info">
                    <span class="current-page">1</span> / <span class="total-pages">{{ math.Ceil (div (len .Pages) 8.0) }}</span>
                </span>
                <button class="pagination-btn next-btn" onclick="showNextPage('{{ .Key }}')">下一页</button>
            </div>
        {{ end }}
    </div>
    {{ end }}

    <style>
        .archives-pagination {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 15px;
            margin: 20px 0;
            padding: 15px;
            background: var(--card-background);
            border-radius: 8px;
        }
        
        .pagination-btn {
            padding: 8px 16px;
            border: 1px solid var(--border-color);
            background: var(--card-background);
            color: var(--primary-color);
            border-radius: 4px;
            cursor: pointer;
            transition: all 0.2s ease;
        }
        
        .pagination-btn:hover:not(:disabled) {
            background: #ff69b4;
            color: white;
        }
        
        .pagination-btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }
        
        .pagination-info {
            font-size: 14px;
            color: var(--secondary-text-color);
        }
    </style>

    <script>
        const paginationState = {};
        
        function initPagination(year) {
            const container = document.querySelector(`[data-year="${year}"]`);
            const totalItems = parseInt(container.dataset.total);
            const itemsPerPage = 8;
            const totalPages = Math.ceil(totalItems / itemsPerPage);
            
            paginationState[year] = {
                currentPage: 1,
                totalPages: totalPages,
                itemsPerPage: itemsPerPage
            };
        }
        
        function showPage(year, page) {
            const container = document.querySelector(`[data-year="${year}"]`);
            const items = container.querySelectorAll('.article-item');
            const startIndex = (page - 1) * 8;
            const endIndex = startIndex + 8;
            
            items.forEach((item, index) => {
                if (index >= startIndex && index < endIndex) {
                    item.style.display = 'block';
                } else {
                    item.style.display = 'none';
                }
            });
            
            // 更新分页控件
            const pagination = document.querySelector(`.archives-pagination[data-year="${year}"]`);
            if (pagination) {
                const currentPageSpan = pagination.querySelector('.current-page');
                const prevBtn = pagination.querySelector('.prev-btn');
                const nextBtn = pagination.querySelector('.next-btn');
                
                currentPageSpan.textContent = page;
                prevBtn.disabled = page === 1;
                nextBtn.disabled = page === paginationState[year].totalPages;
            }
            
            paginationState[year].currentPage = page;
        }
        
        function showNextPage(year) {
            if (paginationState[year] && paginationState[year].currentPage < paginationState[year].totalPages) {
                showPage(year, paginationState[year].currentPage + 1);
            }
        }
        
        function showPrevPage(year) {
            if (paginationState[year] && paginationState[year].currentPage > 1) {
                showPage(year, paginationState[year].currentPage - 1);
            }
        }
        
        // 初始化所有年份的分页
        document.addEventListener('DOMContentLoaded', function() {
            const archivesGroups = document.querySelectorAll('.archives-group');
            archivesGroups.forEach(group => {
                const year = group.querySelector('.article-list--compact').dataset.year;
                if (year) {
                    initPagination(year);
                }
            });
        });
    </script>

    {{ partialCached "footer/footer" . }}
{{ end }}
```
即可！！！