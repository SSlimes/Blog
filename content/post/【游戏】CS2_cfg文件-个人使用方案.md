---
title: 【游戏】CS2_cfg文件-个人使用方案
date: 2025-08-31
categories:
  - 其他
image: images/21.webp
---
# autoexec.cfg
```
alias +pwaswitchknife slot3
alias -pwaswitchknife lastinv
alias "refundall" "sellback 0;sellback 1;sellback 2;sellback 3;sellback 4;sellback 5;sellback 6;sellback 8;sellback 9;sellback 10;sellback 11;sellback 12;sellback 13;sellback 14;sellback 15;sellback 16;sellback 17;sellback 18;sellback 26;sellback 26;sellback 27;sellback 28;sellback 29;sellback 30;sellback 32;sellback 33;sellback 34;sellback 35;play ui\panorama\itemtile_click_02.vsnd_c"

//-----------------------------HUD设置--------------------------------
//游戏HUD(0关闭,1开启)
cl_drawhud "1";

//只显示击杀和准星(0关闭,1开启)
cl_draw_only_deathnotices "0";

//HUD颜色(0队伍颜色,1白色,2亮白色,3淡蓝色,4蓝色,5紫色,6红色,7橙色,8黄色,9绿色,10浅绿色,11粉红色)
cl_hud_color "11";

//一直显示装备栏(0关闭,1开启)
cl_showloadout "1";

//游戏自动提示(0关闭,1开启)
cl_autohelp "0";

//鼠标灵敏度
sensitivity 1.2
zoom_sensitivity_ratio_mouse 1.2

//-----------------------------准星设置--------------------------------
cl_crosshairalpha "200";
cl_crosshaircolor "1";//颜色
cl_crosshairdot "0";
cl_crosshair_t "0";
cl_crosshairgap "-5";//间距
cl_crosshairsize "1.29";//长度
cl_crosshairstyle "4";
cl_crosshairthickness "-10.0";//粗细
cl_crosshair_outlinethickness "0";
cl_crosshair_drawoutline "0";
//狙击准星粗细
cl_crosshair_sniper_width "0";

//狙击准星模糊(0关闭，1开启)
cl_crosshair_sniper_show_normal_inaccuracy "0";

//准星警告(0关闭，1开启)
cl_crosshair_friendly_warning "0";


//手臂视角类型(0自定义，1默认，2写实，3经典)
viewmodel_presetpos "3";

//手臂左右位置(-2.25~2.25)
viewmodel_offset_x "2.5";

//手臂前后位置(-2~2)
viewmodel_offset_y "0";

//手臂上下位置(-2~2)
viewmodel_offset_z "-1.5";

//手臂FOV(54~68)
viewmodel_fov "68";

//新手臂摇晃动作(false关闭，true开启)
cl_usenewbob false

//-----------------------------鼠标按键设置--------------------------------
//鼠标左键 开火
bind "mouse1" "+attack";
//鼠标右键 第二开火
bind "mouse2" "+attack2";
//鼠标里侧键 使用麦克风
bind "mouse5" "+voicerecord";
//鼠标外侧键 切换左右持枪
bind "mouse4" "switchhands";
//鼠标下滚轮 跳跃
bind "mwheeldown" "+jump";

//-----------------------------雷达设置--------------------------------
//游戏雷达(0关闭,1开启)
cl_drawhud_force_radar "1";
//雷达大小(0.8~1.3)
cl_hud_radar_scale "1.3";
//雷达缩放(0.25~0.7)
cl_radar_scale "0.35";
//雷达以玩家为中心(0关闭,1开启)
cl_radar_always_centered "0";
//雷达旋转(0关闭,1开启)
cl_radar_rotate "1";
//雷达人物大小(0.4~1)
cl_radar_icon_scale_min "0.4";
//计分板雷达显示模式(0圆形,1方形)
cl_radar_square_with_scoreboard "1";

//-----------------------------游戏帧数上限(0无上限)--------------------------------
fps_max "0";


//-----------------------------按键绑定--------------------------------
bind "downarrow" "buy rifle1;"
bind "rightarrow" "buy rifle4;"
bind "leftarrow" "buy vesthelm;buy vest;"
bind "kp_3" "buy secondary4;"
bind "kp_4" "buy smokegrenade;"
bind "kp_5" "buy flashbang;"
bind "kp_6" "buy molotov;buy incgrenade;"

//-----------------------------滚轮跳设置--------------------------------
//下滚轮跳
bind "MWHEELDOWN" "+jump"
bind "v" "+jump"
bind "space" "+jump"
```
# lianxi.cfg
一些跑图的cfg文件：
```
//开启作弊
sv_cheats true

//去除开场动画
cl_versus_intro false
mp_team_intro_time 0

//钱 无限时间 购买时间地点
mp_buy_anywhere 1
mp_buytime 99999
ammo_grenade_limit_total 5
mp_maxmoney 16000
mp_startmoney 16000
mp_roundtime 60
mp_roundtime_defuse 60
mp_roundtime_hostage 60
mp_freezetime 0
bot_stop 1

//无坠落道具伤害 能看到火烧效果
sv_falldamage_scale 0
sv_hegrenade_damage_multiplier 0
sv_regeneration_force_on true
ff_damage_reduction_grenade_self 0

//飞行
bind alt noclip

//加bot和踢bot
bind = bot_add
bind - bot_kick

//放置bot
bind o bot_place

//重复上一个道具
bind i sv_rethrow_last_grenade

//刷新游戏
bind l mp_restartgame 1

//切换子弹落点显示
bind p "toggle sv_showimpacts 0 1"

//清除烟雾弹
bind k "ent_fire smokegrenade_projectile kill;ent_fire molotov_projectile kill;ent_fire flashbang_projectile kill;ent_fire hegrenade_projectile kill;ent_fire decoy_projectile kill;stopsound"

//自动复活
mp_respawn_on_death_ct true 
mp_respawn_on_death_t true
mp_restartgame 1
```