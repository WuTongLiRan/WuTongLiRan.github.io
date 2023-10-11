# 

# Argon主题博客美化

## 博客架构

> 我不是专门写 Web 的，造轮子 肯定是做不到了，只得拿 轮子 和 零件 来组装

**框架：**PHPStudy ( Apache + MySQL + PHP )

**CMS：**WordPress

**Theme：**Argon

​	

## 前言

>  我的博客是基于**WordPress**建站系统的**Argon**主题，为同样基础的网站可以直接照抄，不一样基础的可能得需要一些Web开发经验才能运用上本文代码

​	

## Argon 主题选项

**管理员后台 → Argon 主题选项**

点击 **导入设置** ，粘贴JSON代码，记得**做些必要的修改**，如：网站目录、图片链接……

```json
{
  "argon_theme_color": "#516069",
  "argon_theme_color_hex_preview": "#516069",
  "argon_show_customize_theme_color_picker": true,
  "argon_enable_immersion_color": "true",
  "argon_darkmode_autoswitch": "false",
  "argon_enable_amoled_dark": "false",
  "argon_card_radius": "4",
  "argon_card_shadow": "big",
  "argon_page_layout": "double",
  "argon_article_list_waterflow": "2and3",
  "argon_article_list_layout": "1",
  "argon_font": "serif",
  "argon_assets_path": "jsdelivr",
  "argon_custom_assets_path": "",
  "argon_wp_path": "/blog/",
  "argon_dateformat": "YMD",
  "argon_enable_headroom": "false",
  "argon_toolbar_title": " ",
  "argon_toolbar_icon": "http://8.130.91.80/blog/wp-content/uploads/2023/05/retouch_2023052719093000.png",
  "argon_toolbar_icon_link": "http://8.130.91.80/blog/wp-admin/",
  "argon_toolbar_blur": "true",
  "argon_banner_title": "桐漓の博客",
  "argon_banner_subtitle": "TL's BLOG",
  "argon_banner_size": "fullscreen",
  "argon_page_background_banner_style": "transparent",
  "argon_show_toolbar_mask": true,
  "argon_banner_background_url": "",
  "argon_banner_background_color_type": "shape-dark",
  "argon_banner_background_hide_shapes": false,
  "argon_enable_banner_title_typing_effect": "true",
  "argon_banner_typing_effect_interval": "200",
  "argon_page_background_url": "https://pic.imgdb.cn/item/64b4bb101ddac507cc723295.png",
  "argon_page_background_dark_url": "https://pic.imgdb.cn/item/64b4bb101ddac507cc723295.png",
  "argon_page_background_opacity": "1",
  "argon_sidebar_banner_title": "TL's BLOG",
  "argon_sidebar_banner_subtitle": "山腰太挤，顶峰见",
  "argon_sidebar_auther_name": "梧桐漓染 & 七折",
  "argon_sidebar_auther_image": "https://pic.imgdb.cn/item/64a29e5b1ddac507cc038d97.jpg",
  "argon_sidebar_author_description": "Web渗透、Android逆向小白 & 开发小白",
  "argon_sidebar_announcement": "<style>\n  .special-text {\n    font-size: 13px;\n    line-height: 1;\n  }\n\n  .first-line {\nfont-size: 15px;\n    font-weight: bold;\n    color: black;\n  }\n</style>\n<p class=\"special-text first-line\">欢迎来到梧桐漓染的Blog</p>\n<p class=\"special-text\">希望这里的内容能帮到你</p>\n<hr>\n<p class=\"special-text\">最近域名用不了咯，处理中……</p>\n",
  "argon_fab_show_settings_button": "true",
  "argon_fab_show_darkmode_button": "false",
  "argon_fab_show_gotocomment_button": "true",
  "argon_seo_description": "",
  "argon_seo_keywords": "",
  "argon_article_meta": "views|time|categories|author",
  "argon_show_readingtime": "true",
  "argon_reading_speed": "300",
  "argon_reading_speed_en": "160",
  "argon_reading_speed_code": "20",
  "argon_show_thumbnail_in_banner_in_content_page": "false",
  "argon_first_image_as_thumbnail_by_default": "false",
  "argon_reference_list_title": "参考",
  "argon_show_sharebtn": "domestic",
  "argon_show_headindex_number": "false",
  "argon_donate_qrcode_url": "",
  "argon_additional_content_after_post": "文章：%title%\n作者：%author%\n\n此文章如果能帮助到你就已经物超所值了\n",
  "argon_related_post": "category,tag",
  "argon_related_post_sort_orderby": "date",
  "argon_related_post_sort_order": "DESC",
  "argon_related_post_limit": "10",
  "argon_article_header_style": "article-header-style-2",
  "argon_outdated_info_time_type": "modifiedtime",
  "argon_outdated_info_days": "-1",
  "argon_outdated_info_tip_type": "inpost",
  "argon_outdated_info_tip_content": "本文最后更新于 %date_delta% 天前，其中的信息可能已经有所发展或是发生改变。",
  "argon_archives_timeline_show_month": "true",
  "argon_archives_timeline_url": "http://8.130.91.80/wordpress/%e5%bd%92%e6%a1%a3/",
  "argon_footer_html": "<style>\n/* 核心样式 */\n.github-badge {\ndisplay: inline-block;\nborder-radius: 4px;\ntext-shadow: none;\nfont-size: 13.1px;\ncolor: #fff;\nline-height: 15px;\nmargin-bottom: 5px;\nfont-family: \"Open Sans\", sans-serif;\n}\n.github-badge .badge-subject {\ndisplay: inline-block;\nbackground-color: #4d4d4d;\npadding: 4px 4px 4px 6px;\nborder-top-left-radius: 4px;\nborder-bottom-left-radius: 4px;\nfont-family: \"Open Sans\", sans-serif;\n}\n.github-badge .badge-value {\ndisplay: inline-block;\npadding: 4px 6px 4px 4px;\nborder-top-right-radius: 4px;\nborder-bottom-right-radius: 4px;\nfont-family: \"Open Sans\", sans-serif;\n}\n.github-badge-big {\ndisplay: inline-block;\nborder-radius: 6px;\ntext-shadow: none;\nfont-size: 14.1px;\ncolor: #fff;\nline-height: 18px;\nmargin-bottom: 7px;\n}\n.github-badge-big .badge-subject {\ndisplay: inline-block;\nbackground-color: #4d4d4d;\npadding: 4px 4px 4px 6px;\nborder-top-left-radius: 4px;\nborder-bottom-left-radius: 4px;\n}\n.github-badge-big .badge-value {\ndisplay: inline-block;\npadding: 4px 6px 4px 4px;\nborder-top-right-radius: 4px;\nborder-bottom-right-radius: 4px;\n}\n.bg-orange {\nbackground-color: #ec8a64 !important;\n}\n.bg-red {\nbackground-color: #cb7574 !important;\n}\n.bg-apricots {\nbackground-color: #64769e!important;\n}\n.bg-casein {\nbackground-color: #dfe291 !important;\n}\n.bg-shallots {\nbackground-color: #97c3c6 !important;\n}\n.bg-ogling {\nbackground-color: #95c7e0 !important;\n}\n.bg-haze {\nbackground-color: #9aaec7 !important;\n}\n.bg-mountain-terrier {\nbackground-color: #99a5cd !important;\n}\n</style>\n\n\n\t<!-- 运行时间 -->\n    <div class=\"github-badge-big\">\n        <span class=\"badge-subject\"><i class=\"fa fa-clock-o\"></i> 运行时间</span><span\n            class=\"badge-value bg-apricots\"><span id=\"blog_running_days\" class=\"odometer odometer-auto-theme\"></span>\n            days\n            <span id=\"blog_running_hours\" class=\"odometer odometer-auto-theme\"></span> H\n            <span id=\"blog_running_mins\" class=\"odometer odometer-auto-theme\"></span> M\n            <span id=\"blog_running_secs\" class=\"odometer odometer-auto-theme\"></span>S\n        </span>\n\n<script no-pjax=\"\">\nvar blog_running_days = document.getElementById(\"blog_running_days\");\nvar blog_running_hours = document.getElementById(\"blog_running_hours\");\nvar blog_running_mins = document.getElementById(\"blog_running_mins\");\nvar blog_running_secs = document.getElementById(\"blog_running_secs\");\nfunction refresh_blog_running_time() {\nvar time = new Date() - new Date(2023, 4, 23, 0, 0, 0); /*此处日期的月份改为自己真正月份的前一个月*/\nvar d = parseInt(time / 24 / 60 / 60 / 1000);\nvar h = parseInt((time % (24 * 60 * 60 * 1000)) / 60 / 60 / 1000);\nvar m = parseInt((time % (60 * 60 * 1000)) / 60 / 1000);\nvar s = parseInt((time % (60 * 1000)) / 1000);\nblog_running_days.innerHTML = d;\nblog_running_hours.innerHTML = h;\nblog_running_mins.innerHTML = m;\nblog_running_secs.innerHTML = s;\n}\nrefresh_blog_running_time();\nif (typeof bottomTimeIntervalHasSet == \"undefined\") {\nvar bottomTimeIntervalHasSet = true;\nsetInterval(function () {\nrefresh_blog_running_time();\n}, 500);\n}\n</script>\n</div>\n<div>\n<style>\n@keyframes jump {\n    0% { transform: translateY(0) scale(1); }\n    50% { transform: translateY(-5px) scale(0.9); }\n    100% { transform: translateY(0) scale(1); }\n}\n\n.jumping-text span {\n    display: inline-block;\n    animation: jump 2s infinite;\n    animation-delay: 0s;\n    will-change: transform; /* 添加优化提示 */\n}\n\n\n.jumping-text span:nth-child(1) {\n    animation-delay: 0s;\n}\n\n.jumping-text span:nth-child(2) {\n    animation-delay: 0.2s;\n}\n\n.jumping-text span:nth-child(3) {\n    animation-delay: 0.4s;\n}\n\n/* Add more nth-child styles for each letter */\n\n</style>\n</head>\n<body>\n<i class=\"fa fa-wordpress\"></i>\n<scan class=\"jumping-text\">\n<span>博</span>\n    <span>客</span>\n    <span>基</span>\n    <span>于</span>\n    <span>Argon</span>\n    <span>主</span>\n    <span>题</span>\n    <span>二</span>\n    <span>次</span>\n    <span>开</span>\n    <span>发</span>\n    <span>🛠️</span>\n</scan>\n\n<script>\n    const jumpingText = document.querySelector('.jumping-text');\n    const letters = jumpingText.querySelectorAll('span');\n\n    let delay = 0;\n    letters.forEach((letter) => {\n        letter.style.animationDelay = delay + 's';\n        delay += 0.1;\n    });\n</script>\n</div>\n\n\n<style>\n    .badge-1 {\n        font-size: 9px; /* 调整为您想要的尺寸 */\n    }\n</style>\n\n<span class=\"badge-1\">点下方会跳到Argon项目地址(跳过去就不是我的网站哦不要搞错了•ࡇ•!)</span>",
  "argon_enable_code_highlight": "true",
  "argon_code_theme": "androidstudio",
  "argon_code_highlight_hide_linenumber": "false",
  "argon_code_highlight_break_line": "false",
  "argon_code_highlight_transparent_linenumber": "false",
  "argon_math_render": "mathjax2",
  "argon_mathjax_cdn_url": "https://cdn.bootcdn.net/ajax/libs/mathjax/3.2.2/es5/a11y/assistive-mml.js",
  "argon_mathjax_v2_cdn_url": "https://cdn.bootcdn.net/ajax/libs/mathjax/2.7.9/MathJax.js?config=TeX-AMS_HTML",
  "argon_katex_cdn_url": "//cdn.jsdelivr.net/npm/katex@0.11.1/dist/",
  "argon_enable_lazyload": "true",
  "argon_lazyload_threshold": "800",
  "argon_lazyload_effect": "fadeIn",
  "argon_lazyload_loading_style": "7",
  "argon_enable_fancybox": "true",
  "argon_enable_zoomify": "false",
  "argon_zoomify_duration": "200",
  "argon_zoomify_easing": "cubic-bezier(0.4,0,0,1)",
  "argon_zoomify_scale": "0.9",
  "argon_enable_pangu": "false",
  "argon_custom_html_head": "",
  "argon_custom_html_foot": "",
  "argon_enable_smoothscroll_type": "1_pulse",
  "argon_enable_into_article_animation": "true",
  "argon_disable_pjax_animation": "false",
  "argon_comment_pagination_type": "page",
  "argon_comment_emotion_keyboard": "true",
  "argon_hide_name_email_site_input": "false",
  "argon_comment_need_captcha": "false",
  "argon_get_captcha_by_ajax": "false",
  "argon_comment_allow_markdown": "true",
  "argon_comment_allow_editing": "true",
  "argon_comment_allow_privatemode": "true",
  "argon_comment_allow_mailnotice": "true",
  "argon_comment_mailnotice_checkbox_checked": true,
  "argon_comment_enable_qq_avatar": "true",
  "argon_comment_avatar_vcenter": "false",
  "argon_who_can_visit_comment_edit_history": "admin",
  "argon_enable_comment_pinning": "true",
  "argon_enable_comment_upvote": "false",
  "argon_comment_ua": "platform,browser,version",
  "argon_show_comment_parent_info": "true",
  "argon_fold_long_comments": "true",
  "argon_gravatar_cdn": "gravatar.pho.ink/avatar/",
  "argon_text_gravatar": "false",
  "argon_enable_search_filters": "true",
  "argon_search_filters_type": "*post,*page,shuoshuo",
  "argon_pjax_disabled": "false",
  "argon_hide_categories": "",
  "argon_enable_login_css": "true",
  "argon_home_show_shuoshuo": "false",
  "argon_fold_long_shuoshuo": "false",
  "argon_enable_timezone_fix": "false",
  "argon_hide_shortcode_in_preview": "false",
  "argon_trim_words_count": "0",
  "argon_enable_mobile_scale": "false",
  "argon_disable_googlefont": "false",
  "argon_disable_codeblock_style": "false",
  "argon_update_source": "stop",
  "argon_hide_footer_author": "true"
}
```

​	

## 设置网站字体

**管理员后台 → 外观 → 自定义 → 额外CSS**

```css
/* 设置网站字体*/
@font-face{
	font-family:TL;
    src:
    url(https://fastly.jsdelivr.net/gh/huangwb8/bloghelper@latest/fonts/13.woff2) format('woff2')
}

body{
    font-family:"TL" !important
}
```

​	

## 网站横幅字体彩色霓虹效果 & 鼠标停放时放大

**管理员后台 → 外观 → 自定义 → 额外CSS**

```css
/*网站横幅字体彩色霓虹效果*/
@keyframes ColdLight {
  0% {
    background-position: 0%;
  }
  100% {
    background-position: 200%;
  }
}

.banner-title {
  position: absolute;
  background: linear-gradient(90deg, #03a9f4, #f441a5, #ffeb3b, #03a9f4);
  background-size: 200%;
  animation: ColdLight 3s linear infinite;
  color: transparent !important;
  -webkit-background-clip: text;
  font-weight: 1000; /* 新增加粗字体效果 */
  font-size: 3.5em; /* 横幅字体大小 */
  transition: font-size 0.3s ease; /* 添加过渡效果，时间为0.3秒，缓动方式为ease */
}

/* 横幅字体大小 - 鼠标悬停时 */
.banner-title:hover {
  font-size: 4.6em; /* 鼠标悬停时的字体大小 */
}

.banner-title .banner-title-inner {
  position: relative;
  background: inherit;
}

.banner-title .banner-subtitle {
  position: relative;
  background: inherit;
  font-size: 27px; /* 横幅副标题字体大小 */
}
```

​	

## 一些有关 字体 & 颜色 & 透明 & 排版 & 样式 的设置

**管理员后台 → 外观 → 自定义 → 额外CSS**

```css
/*尾注字体大小*/
.additional-content-after-post{
    font-size: 1.2rem
}

/* 公告居中 */
.leftbar-announcement-title {
    font-size: 20px;
    text-align: center;
}
 
.leftbar-announcement-content {
    font-size: 15px;
    line-height: 1.8;
    padding-top: 8px;
    opacity: 0.9;
    text-align: center;
}

/* 一言居中 */
.leftbar-banner-title {
    font-size: 20px;
    display: block;
    text-align: center;
}

 
/*文章标题字体大小*/
.post-title {
    font-size: 23px
}
 
/*正文字体大小（不包含代码）*/
.post-content p{
    font-size: 1.2rem;
}
li{
    font-size: 1.2rem;
	
}
 
/*评论区字体大小*/
p {
    font-size: 1.2rem
			
}
 
/*评论发送区字体大小*/
.form-control{
    font-size: 1.2rem
}
 
/*评论勾选项目字体大小*/
.custom-checkbox .custom-control-input~.custom-control-label{
    font-size: 1rem
}

/*评论区代码的强调色*/
code {
  color: rgba(var(--themecolor-rgbstr));
}
 
/*说说字体大小和颜色设置*/
.shuoshuo-title {
    font-size: 25px;
/*  color: rgba(var(--themecolor-rgbstr)); */
}
 

/* 个性签名居中 */
.leftbar-banner-subtitle {
    margin-top: 15px;
    margin-bottom: 8px;
    font-size: 13px;
    opacity: 0.8;
    display: block;
    text-align: center;
}

/*文章或页面的正文颜色*/
body{
    color:#364863
}
 
/*引文属性设置*/
blockquote {
    /*添加弱主题色为背景色*/
    background: rgba(var(--themecolor-rgbstr), 0.1) !important;
    width: 100%
}
 
/*引文颜色 建议用主题色*/
:root {
    /*也可以用类似于--color-border-on-foreground-deeper: #009688;这样的命令*/
    --color-border-on-foreground-deeper:#009688;
}

.leftbar-menu-item > a:hover, .leftbar-menu-item.current > a{
    background-color: #f9f9f980;
}

/*站点概览分隔线颜色修改*/
.site-state-item{
    border-left: 1px solid #aaa;
}
.site-friend-links-title {
    border-top: 1px dotted #aaa;
}
#leftbar_tab_tools ul li {
    padding-top: 3px;
    padding-bottom: 3px;
    border-bottom:none;
}
html.darkmode #leftbar_tab_tools ul li {
    border-bottom:none;
}
 
/*左侧栏搜索框的颜色*/
button#leftbar_search_container {
    background-color: transparent;
}

/*白天模式下白色卡片透明度*/
.card{
    background-color:rgba(255, 255, 255, 0.85) !important;
    /*backdrop-filter:blur(6px);*//*毛玻璃效果主要属性*/
    -webkit-backdrop-filter:blur(6px);
}

/*小工具栏背景完全透明*/
/*小工具栏是card的子元素，如果用同一个透明度会叠加变色，故改为完全透明*/
.card .widget,.darkmode .card .widget,#post_content > div > div > div.argon-timeline-card.card.bg-gradient-secondary.archive-timeline-title{
    background-color:#ffffff00 !important;
    backdrop-filter:none;
    -webkit-backdrop-filter:none;
}
.emotion-keyboard,#fabtn_blog_settings_popup{
    background-color:rgba(255, 255, 255, 0.95) !important;
}

/*分类卡片透明*/
.bg-gradient-secondary{
    background:rgba(255, 255, 255, 0.5) !important;
    backdrop-filter: blur(2px);
    -webkit-backdrop-filter:blur(10px);
}

/*夜间模式透明*/
html.darkmode.bg-white,html.darkmode .card,html.darkmode #footer{
    background:rgba(66, 66, 66, 0.85) !important;
}
html.darkmode #fabtn_blog_settings_popup{/*这个我也忘了是什么设置什么的透明度*/
    background:rgba(66, 66, 66, 0.95) !important;
}

/*========排版设置===========*/
 
/*左侧栏层级置于上层*/
#leftbar_part1 {
    z-index: 1;
}

/*分类卡片文本居中*/
#content > div.page-information-card-container > div > div{
    text-align:center;
}

/*子菜单对齐及样式调整*/
.dropdown-menu .dropdown-item>i{
    width: 10px;
}
.dropdown-menu>a {
    color:var(--themecolor);
}
.dropdown-menu{
    min-width:max-content;
}
.dropdown-menu .dropdown-item {
    padding: .5rem 1.5rem 0.5rem 1rem;
}
.leftbar-menu-subitem{
    min-width:max-content;
}
.leftbar-menu-subitem .leftbar-menu-item>a{
    padding: 0rem 1.5rem 0rem 1rem;
}

/*左侧栏边距修改*/
.tab-content{
    padding:10px 0px 0px 0px !important;
}
.site-author-links{
    padding:0px 0px 0px 10px ;
}

/*目录位置偏移修改*/
#leftbar_catalog{
    margin-left: 0px;
}

/*目录条目边距修改*/
#leftbar_catalog .index-link{
    padding: 4px 4px 4px 4px;
}

/*左侧栏小工具栏字体缩小*/
#leftbar_tab_tools{
    font-size: 14px;
}
 
/*正文图片边距修改*/
article figure {margin:0;}

/*正文图片居中显示*/
.fancybox-wrapper {
    margin: auto;
}

/*正文表格样式修改*/
article table > tbody > tr > td,
article table > tbody > tr > th,
article table > tfoot > tr > td,
article table > tfoot > tr > th,
article table > thead > tr > td,
article table > thead > tr > th{
    padding: 8px 10px;
    border: 1px solid;
}

/*表格居中样式*/
.wp-block-table.aligncenter{margin:10px auto;}
 
/*回顶图标放大*/
button#fabtn_back_to_top, button#fabtn_go_to_comment, button#fabtn_toggle_blog_settings_popup, button#fabtn_toggle_sides, button#fabtn_open_sidebar{
    font-size: 1.2rem;
}
 
/*顶栏菜单放大*/
 
 
.navbar-nav .nav-link {
    font-size: 1rem;
    font-family: 'echo';
			
}
.navbar-brand {
	color: white!important;
	font-family: 'echo';
    font-size: 1.4rem;
    margin-right: 1rem;
    padding-bottom: .2rem;
}

/*菜单大小*/
.nav-link-inner--text {
    font-size: 1.2em;
}
.navbar-nav .nav-item {
    margin-right:0;
}
.mr-lg-5, .mx-lg-5 {
    margin-right:1rem !important;
}
.navbar-toggler-icon {
    width: 1.8rem;
    height: 1.8rem;
}

/*菜单间距*/
.navbar-expand-lg .navbar-nav .nav-link {
    padding-right: 2.1em;
    padding-left: 1.em;
}

/* Github卡片样式*/
.github-info-card-header a {
    /*Github卡片抬头颜色*/
    color: black !important;
    font-size: 1.5rem;
}
.github-info-card {
    /*Github卡片文字（非链接）*/
    font-size: 1rem;
    color: black !important;
}
.github-info-card.github-info-card-full.card.shadow-sm {
    /*Github卡片背景色*/
    background-color: rgba(var(--themecolor-rgbstr), 0.1) !important;
}

/*导航栏图标大小*/
.navbar-brand img {
height: 40px;
}

/*左上角图片和右侧文字的间距*/
.mr-lg-5, .mx-lg-5 {
margin-right: 3rem !important;
}
```

​	

## banner下方小箭头滚动效果

1. **管理员后台 → 外观 → 自定义 → 额外CSS**

```css
/*banner下方小箭头滚动效果*/
@keyframes up-down-move {
0% {
opacity:0;
transform:translate(-50%,-150px); 
}
50% {
opacity:1;
transform:translate(-50%,-130px); 
}
100% {
opacity:0;
transform:translate(-50%,-110px); 
}
}
 
.cover-scroll-down .fa-angle-down{
font-size: 3rem;
text-shadow: 0px 0px 8px #dc1111;
position:absolute;
transform: translate(-50%,-80px);
opacity:0;
}
 
.cover-scroll-down #pointer1{
animation: up-down-move 3s linear infinite;
 
}
 
.cover-scroll-down #pointer2{
animation: up-down-move 3s 1s linear infinite;
}
 
.cover-scroll-down #pointer3{
animation: up-down-move 3s 2s linear infinite;
}
```

2. **管理员后台 → 外观 → 主题文件编辑器 → 主题页眉(header.php文件)**

3. 439行`<div class="cover-scroll-down">`的下一行(`<i>`标签的那行)替换成以下三行代码

   ```php+HTML
   <i class="fa fa-angle-down" aria-hidden="true" id="pointer1"></i>
   <i class="fa fa-angle-down" aria-hidden="true" id="pointer2"></i>
   <i class="fa fa-angle-down" aria-hidden="true" id="pointer3"></i>
   ```

​	

##  鼠标停放头像时旋转缩放 & 亮暗

**管理员后台 → 外观 → 自定义 → 额外CSS**

```css
#leftbar_overview_author_image {
    width: 100px;
    height: 100px;
    margin: auto;
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
    background-color: rgba(127, 127, 127, 0.1);
    overflow: hidden;
    box-shadow: 0 0 5px rgba(116, 8, 204, 0.3);
    transition: transform 0.3s ease; /*变化速度*/
}
 
#leftbar_overview_author_image {
	transition: transform 0.3s ease-in-out; /* 添加过渡效果 */
}

#leftbar_overview_author_image:hover {
	transform: scale(1.3) rotate(360deg); /* 缩放并旋转 */
}
```

​	

## 点击头像 & 姓名跳转相关页面

**管理员后台 → 外观 → 主题文件编辑器 → 边栏文件(sidebar.php)**

点击 头像 跳转大概在第 126 行左右

```php+HTML
<div class="tab-pane fade text-center<?php if ($nowActiveTab == 1) { echo ' active show'; }?>" id="leftbar_tab_overview"
     role="tabpanel" aria-labelledby="leftbar_tab_overview_btn">
/*添加的代码，链接是要跳转的页面链接*/    <a href="http://8.130.91.80/blog/?p=97">
        <div id="leftbar_overview_author_image"
             style="background-image: url(<?php echo get_option('argon_sidebar_auther_image') == '' ? 'data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz48c3ZnIHZlcnNpb249IjEuMSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB4bWxuczp4bGluaz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayIgeD0iMHB4IiB5PSIwcHgiIHZpZXdCb3g9IjAgMCAxMDAgMTAwIiB4bWw6c3BhY2U9InByZXNlcnZlIj48cmVjdCBmaWxsPSIjNUU3MkU0MjIiIHdpZHRoPSIxMDAiIGhlaWdodD0iMTAwIi8+PGc+PGcgb3BhY2l0eT0iMC4zIj48cGF0aCBmaWxsPSIjNUU3MkU0IiBkPSJNNzQuMzksMzIuODZjLTAuOTgtMS43LTMuMzktMy4wOS01LjM1LTMuMDlINDUuNjJjLTEuOTYsMC00LjM3LDEuMzktNS4zNSwzLjA5TDI4LjU3LDUzLjE1Yy0wLjk4LDEuNy0wLjk4LDQuNDgsMCw2LjE3bDExLjcxLDIwLjI5YzAuOTgsMS43LDMuMzksMy4wOSw1LjM1LDMuMDloMjMuNDNjMS45NiwwLDQuMzctMS4zOSw1LjM1LTMuMDlMODYuMSw1OS4zMmMwLjk4LTEuNywwLjk4LTQuNDgsMC02LjE3TDc0LjM5LDMyLjg2eiIvPjwvZz48ZyBvcGFjaXR5PSIwLjgiPjxwYXRoIGZpbGw9IiM1RTcyRTQiIGQ9Ik02Mi4wNCwyMC4zOWMtMC45OC0xLjctMy4zOS0zLjA5LTUuMzUtMy4wOUgzMS43M2MtMS45NiwwLTQuMzcsMS4zOS01LjM1LDMuMDlMMTMuOSw0Mi4wMWMtMC45OCwxLjctMC45OCw0LjQ4LDAsNi4xN2wxMi40OSwyMS42MmMwLjk4LDEuNywzLjM5LDMuMDksNS4zNSwzLjA5aDI0Ljk3YzEuOTYsMCw0LjM3LTEuMzksNS4zNS0zLjA5bDEyLjQ5LTIxLjYyYzAuOTgtMS43LDAuOTgtNC40OCwwLTYuMTdMNjIuMDQsMjAuMzl6Ii8+PC9nPjwvZz48L3N2Zz4=' : get_option('argon_sidebar_auther_image'); ?>)"
             class="rounded-circle shadow-sm" alt="avatar"></div>
/*添加的代码*/    </a>
```

点击 姓名 跳转则是 130 行(添加完上面代码的情况下)左右

```php+HTML
<a href="http://8.130.91.80/blog/?p=97">/*添加的代码*/
    <h6 id="leftbar_overview_author_name"><?php echo get_option('argon_sidebar_auther_name') == '' ? bloginfo('name') : get_option('argon_sidebar_auther_name'); ?></h6>
</a>/*添加的代码*/
```

## 网站底部运行时间等信息

**管理员后台 → Argon 主题选项 → 页脚内容**

```php+HTML
<style>
    /* 核心样式 */
    .github-badge {
        display: inline-block;
        border-radius: 4px;
        text-shadow: none;
        font-size: 13.1px;
        color: #fff;
        line-height: 15px;
        margin-bottom: 5px;
        font-family: "Open Sans", sans-serif;
    }

    .github-badge .badge-subject {
        display: inline-block;
        background-color: #4d4d4d;
        padding: 4px 4px 4px 6px;
        border-top-left-radius: 4px;
        border-bottom-left-radius: 4px;
        font-family: "Open Sans", sans-serif;
    }

    .github-badge .badge-value {
        display: inline-block;
        padding: 4px 6px 4px 4px;
        border-top-right-radius: 4px;
        border-bottom-right-radius: 4px;
        font-family: "Open Sans", sans-serif;
    }

    .github-badge-big {
        display: inline-block;
        border-radius: 6px;
        text-shadow: none;
        font-size: 14.1px;
        color: #fff;
        line-height: 18px;
        margin-bottom: 7px;
    }

    .github-badge-big .badge-subject {
        display: inline-block;
        background-color: #4d4d4d;
        padding: 4px 4px 4px 6px;
        border-top-left-radius: 4px;
        border-bottom-left-radius: 4px;
    }

    .github-badge-big .badge-value {
        display: inline-block;
        padding: 4px 6px 4px 4px;
        border-top-right-radius: 4px;
        border-bottom-right-radius: 4px;
    }

    .bg-orange {
        background-color: #ec8a64 !important;
    }

    .bg-red {
        background-color: #cb7574 !important;
    }

    .bg-apricots {
        background-color: #64769e !important;
    }

    .bg-casein {
        background-color: #dfe291 !important;
    }

    .bg-shallots {
        background-color: #97c3c6 !important;
    }

    .bg-ogling {
        background-color: #95c7e0 !important;
    }

    .bg-haze {
        background-color: #9aaec7 !important;
    }

    .bg-mountain-terrier {
        background-color: #99a5cd !important;
    }
</style>


<!-- 运行时间 -->
<div class="github-badge-big">
    <span class="badge-subject"><i class="fa fa-clock-o"></i> 运行时间</span><span
        class="badge-value bg-apricots"><span id="blog_running_days" class="odometer odometer-auto-theme"></span>
            days
            <span id="blog_running_hours" class="odometer odometer-auto-theme"></span> H
            <span id="blog_running_mins" class="odometer odometer-auto-theme"></span> M
            <span id="blog_running_secs" class="odometer odometer-auto-theme"></span>S
        </span>

    <script no-pjax="">
        var blog_running_days = document.getElementById("blog_running_days");
        var blog_running_hours = document.getElementById("blog_running_hours");
        var blog_running_mins = document.getElementById("blog_running_mins");
        var blog_running_secs = document.getElementById("blog_running_secs");

        function refresh_blog_running_time() {
            var time = new Date() - new Date(2023, 4, 23, 0, 0, 0); /*此处日期的月份改为自己真正月份的前一个月*/
            var d = parseInt(time / 24 / 60 / 60 / 1000);
            var h = parseInt((time % (24 * 60 * 60 * 1000)) / 60 / 60 / 1000);
            var m = parseInt((time % (60 * 60 * 1000)) / 60 / 1000);
            var s = parseInt((time % (60 * 1000)) / 1000);
            blog_running_days.innerHTML = d;
            blog_running_hours.innerHTML = h;
            blog_running_mins.innerHTML = m;
            blog_running_secs.innerHTML = s;
        }

        refresh_blog_running_time();
        if (typeof bottomTimeIntervalHasSet == "undefined") {
            var bottomTimeIntervalHasSet = true;
            setInterval(function () {
                refresh_blog_running_time();
            }, 500);
        }
    </script>
</div>
<div>
    <style>
        @keyframes jump {
            0% {
                transform: translateY(0) scale(1);
            }
            50% {
                transform: translateY(-5px) scale(0.9);
            }
            100% {
                transform: translateY(0) scale(1);
            }
        }

        .jumping-text span {
            display: inline-block;
            animation: jump 2s infinite;
            animation-delay: 0s;
            will-change: transform; /* 添加优化提示 */
        }


        .jumping-text span:nth-child(1) {
            animation-delay: 0s;
        }

        .jumping-text span:nth-child(2) {
            animation-delay: 0.2s;
        }

        .jumping-text span:nth-child(3) {
            animation-delay: 0.4s;
        }

        /* Add more nth-child styles for each letter */

    </style>
    </head>
    <body>
    <i class="fa fa-wordpress"></i>
    <scan class="jumping-text">
        <span>博</span>
        <span>客</span>
        <span>基</span>
        <span>于</span>
        <span>Argon</span>
        <span>主</span>
        <span>题</span>
        <span>二</span>
        <span>次</span>
        <span>开</span>
        <span>发</span>
        <span>🛠️</span>
    </scan>

    <script>
        const jumpingText = document.querySelector('.jumping-text');
        const letters = jumpingText.querySelectorAll('span');

        let delay = 0;
        letters.forEach((letter) => {
            letter.style.animationDelay = delay + 's';
            delay += 0.1;
        });
    </script>
</div>


<style>
    .badge-1 {
        font-size: 9px; /* 调整为您想要的尺寸 */
    }
</style>

<span class="badge-1">点下方会跳到Argon项目地址(跳过去就不是我的网站哦不要搞错了•ࡇ•!)</span>
```

​	

## 参考文章

想获得更多美化教程建议去看看这些文章 ⬇

[Argon主题博客美化 – Echo (liveout.cn)](https://www.liveout.cn/25/)

[Argon主题美化 - 北冥红烧鱼的芥子空间 (hongshaoyv.com)](https://blog.hongshaoyv.com/argon-beautification/)

[Docker系列 WordPress系列 特效 - Bensz (hwb0307.com)](https://blognas.hwb0307.com/linux/docker/744#comment-918)

[Argon 主题修改记录 - 鸦鸦的巢穴 (crowya.com)](https://crowya.com/681)
