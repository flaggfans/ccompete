

<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>杨水才精神之旅 · 小车不倒只管推 (高德地图版)</title>
    
    <!-- 高德地图 JS API 2.0 - 配置您的Key和安全密钥 -->
    <script src="https://webapi.amap.com/maps?v=2.0&key=bc495121eef36ffc1ae9f327a740bd17&plugin=AMap.Scale,AMap.ToolBar"></script>
    <script type="text/javascript">
        window._AMapSecurityConfig = {
            securityJsCode: '03acff8c548fe23a4f95a7edfd7e4ac2'
        };
    </script>
    
    <!-- Font Awesome 6 -->
    <link rel="stylesheet" href="https://cdn.bootcdn.net/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <!-- 轻量级图表库 Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
    
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'PingFang SC', 'Microsoft YaHei', 'WenQuanYi Micro Hei', sans-serif;
        }
        
        :root {
            --primary: #b31b1b;
            --primary-dark: #8b0000;
            --gold: #c5a028;
            --gold-light: #e5c87b;
            --bg-light: #faf7f2;
            --bg-paper: #f3eee6;
            --text-dark: #2c1e1a;
            --text-soft: #5d4a3a;
            --border-color: #d4b28c;
        }
        
        body {
            background-color: var(--bg-light);
            color: var(--text-dark);
            overflow-x: hidden;
        }
        
        /* 背景纹理 */
        body::before {
            content: '';
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background-image: 
                radial-gradient(circle at 30% 40%, rgba(197, 160, 40, 0.03) 0%, transparent 30%),
                radial-gradient(circle at 70% 60%, rgba(179, 27, 27, 0.03) 0%, transparent 30%);
            pointer-events: none;
            z-index: -1;
        }

        .header {
            background: rgba(255, 255, 255, 0.92);
            backdrop-filter: blur(10px);
            border-bottom: 3px solid var(--gold);
            padding: 8px 0;
            position: sticky;
            top: 0;
            z-index: 1000;
            box-shadow: 0 4px 20px rgba(0,0,0,0.1);
        }
        
        .header-container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 0 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
        }
        
        .header h1 {
            font-size: 1.5rem;
            font-weight: 700;
            display: flex;
            align-items: center;
            gap: 5px;
        }
        
        .header h1 .title-main {
            background: linear-gradient(135deg, var(--primary-dark) 0%, var(--primary) 80%);
            -webkit-background-clip: text;
            background-clip: text;
            -webkit-text-fill-color: transparent;
            color: var(--primary);
        }
        
        .header h1 .title-sub {
            font-size: 1rem;
            color: var(--gold);
            font-weight: 400;
        }
        
        .header-right {
            display: flex;
            gap: 8px;
        }
        
        .share-btn {
            background: var(--primary);
            color: white;
            padding: 5px 15px;
            border-radius: 40px;
            font-weight: 600;
            font-size: 0.9rem;
            cursor: pointer;
            border: 1px solid var(--gold);
            transition: 0.2s;
            display: flex;
            align-items: center;
            gap: 5px;
        }
        
        .share-btn:hover {
            background: var(--primary-dark);
            transform: translateY(-2px);
        }

        /* 主地图区 */
        .map-stage {
            width: 100%;
            height: 85vh;
            position: relative;
            overflow: hidden;
            border-radius: 0 0 30px 30px;
            border-bottom: 2px solid var(--gold);
        }
        
        #map {
            width: 100%;
            height: 100%;
            background: var(--bg-paper);
        }
        
        /* 地图加载失败提示 */
        .map-error-message {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: white;
            padding: 20px;
            border-radius: 10px;
            border: 2px solid var(--primary);
            z-index: 1000;
            text-align: center;
            display: none;
        }
        .map-error-message.show {
            display: block;
        }

        /* 左侧主面板 */
        .story-panel {
            position: absolute;
            top: 10px;
            left: 10px;
            width: 320px;
            background: rgba(250, 247, 242, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 16px;
            padding: 15px;
            z-index: 600;
            box-shadow: 0 5px 15px rgba(0,0,0,0.15);
            border: 1px solid var(--gold);
            max-height: calc(85vh - 100px);
            overflow-y: auto;
        }
        
        .story-panel h2 {
            font-size: 1.4rem;
            color: var(--primary);
            border-left: 4px solid var(--gold);
            padding-left: 8px;
            margin-bottom: 10px;
        }
        
        .story-panel h2 small {
            font-size: 0.85rem;
            color: var(--text-soft);
            display: block;
        }
        
        .old-photo {
            width: 100%;
            height: 200px;
            border-radius: 10px;
            margin: 12px 0;
            border: 3px solid var(--gold);
            overflow: hidden;
            position: relative;
            box-shadow: 0 6px 12px rgba(0,0,0,0.25);
            cursor: pointer;
            transition: transform 0.2s;
        }
        
        .old-photo:hover {
            transform: scale(1.02);
        }
        
        .old-photo img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        
        .photo-caption {
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            background: linear-gradient(transparent, rgba(0,0,0,0.9));
            color: white;
            padding: 25px 8px 8px;
            font-size: 0.85rem;
            text-align: center;
            font-weight: 500;
            letter-spacing: 0.5px;
        }
        
        .quote-box {
            background: rgba(197, 160, 40, 0.1);
            border-left: 3px solid var(--gold);
            padding: 10px;
            border-radius: 8px;
            margin: 10px 0;
            font-style: italic;
        }
        
        .quote-box p {
            font-size: 0.95rem;
            color: var(--text-dark);
        }
        
        .stats-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 8px;
            margin: 12px 0;
        }
        
        .stat-item {
            background: white;
            border-radius: 12px;
            padding: 10px 5px;
            text-align: center;
            box-shadow: 0 2px 8px rgba(0,0,0,0.05);
            border: 1px solid var(--gold-light);
        }
        
        .stat-value {
            font-size: 1.4rem;
            font-weight: 800;
            color: var(--primary);
            line-height: 1.2;
            transition: all 0.3s;
        }
        
        .stat-label {
            font-size: 0.75rem;
            color: var(--text-soft);
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 3px;
        }
        
        .section-title {
            font-size: 1.1rem;
            font-weight: 700;
            color: var(--primary);
            margin: 15px 0 8px;
            padding-bottom: 4px;
            border-bottom: 2px solid var(--gold-light);
            display: flex;
            align-items: center;
            gap: 5px;
        }
        
        .timeline-list {
            margin: 5px 0 10px;
        }
        
        .timeline-item {
            display: flex;
            align-items: flex-start;
            gap: 10px;
            padding: 6px 0;
            border-bottom: 1px dashed rgba(197,160,40,0.2);
        }
        
        .timeline-year {
            background: var(--primary);
            color: white;
            padding: 3px 8px;
            border-radius: 14px;
            font-weight: 700;
            font-size: 0.8rem;
            min-width: 52px;
            text-align: center;
        }
        
        .timeline-content {
            flex: 1;
            font-size: 0.9rem;
            line-height: 1.5;
        }
        
        .timeline-content strong {
            color: var(--primary-dark);
        }
        
        .story-card {
            background: rgba(255, 255, 255, 0.7);
            border-radius: 10px;
            padding: 10px 12px;
            margin: 8px 0;
            border-left: 3px solid var(--gold);
            font-size: 0.9rem;
            line-height: 1.5;
        }
        
        .story-card i {
            color: var(--primary);
            margin-right: 6px;
        }
        
        .story-highlight {
            background: rgba(179, 27, 27, 0.05);
            border-radius: 8px;
            padding: 8px 12px;
            margin: 8px 0;
            font-style: italic;
            color: var(--primary-dark);
            font-weight: 500;
        }
        
        .milestone-list {
            margin-top: 10px;
        }
        
        .milestone-item {
            display: flex;
            align-items: center;
            gap: 10px;
            padding: 5px 0;
            cursor: pointer;
            transition: 0.2s;
            border-radius: 4px;
        }
        
        .milestone-item:hover {
            background: rgba(197,160,40,0.1);
            padding-left: 8px;
        }
        
        .milestone-year {
            background: var(--primary);
            color: white;
            padding: 3px 8px;
            border-radius: 14px;
            font-weight: 700;
            font-size: 0.8rem;
            min-width: 48px;
            text-align: center;
        }
        
        .milestone-desc {
            font-size: 0.85rem;
            color: var(--text-soft);
        }
        
        .milestone-desc strong {
            color: var(--primary-dark);
        }
        
        .walking-status {
            background: var(--bg-paper);
            border-radius: 30px;
            padding: 8px 12px;
            margin-top: 15px;
            display: flex;
            align-items: center;
            gap: 10px;
            border: 1px solid var(--gold);
            font-size: 0.85rem;
        }
        
        .walking-status i {
            color: var(--primary);
            animation: walk 1s infinite;
        }
        
        @keyframes walk {
            0%, 100% { transform: translateX(0); }
            50% { transform: translateX(5px); }
        }

        /* 右侧视频面板 */
        .video-panel {
            position: absolute;
            top: 10px;
            right: 10px;
            width: 260px;
            background: rgba(250, 247, 242, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 12px;
            padding: 10px;
            z-index: 650;
            border: 2px solid var(--gold);
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
        }
        
        .video-panel h4 {
            font-size: 1rem;
            margin-bottom: 8px;
            color: var(--primary);
            display: flex;
            align-items: center;
            gap: 5px;
        }
        
        .video-container {
            width: 100%;
            border-radius: 8px;
            overflow: hidden;
            background: #000;
        }
        
        .video-container video {
            width: 100%;
            height: auto;
            display: block;
        }
        
        .video-source {
            font-size: 0.7rem;
            margin-top: 5px;
            color: #27ae60;
            text-align: center;
        }

        /* 右侧数据看板 */
        .dashboard-panel {
            position: absolute;
            bottom: 140px;
            right: 10px;
            width: 250px;
            background: rgba(250, 247, 242, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 16px;
            padding: 12px;
            z-index: 550;
            border: 1px solid var(--gold);
        }
        
        .dashboard-panel h3 {
            font-size: 1rem;
            color: var(--primary);
            margin-bottom: 8px;
            display: flex;
            align-items: center;
            gap: 5px;
            border-bottom: 1px solid var(--gold-light);
            padding-bottom: 5px;
        }
        
        .chart-container {
            height: 80px;
            margin-bottom: 10px;
        }
        
        .data-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 4px 0;
            font-size: 0.85rem;
            border-bottom: 1px dashed rgba(197,160,40,0.2);
        }
        
        .data-label {
            display: flex;
            align-items: center;
            gap: 5px;
            color: var(--text-soft);
        }
        
        .data-value {
            font-weight: 700;
            color: var(--primary-dark);
            transition: all 0.3s;
        }
        
        .progress-bar {
            height: 6px;
            background: #e0d5c5;
            border-radius: 10px;
            margin: 5px 0 10px;
            overflow: hidden;
        }
        
        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, var(--gold), var(--primary));
            width: 0%;
            transition: width 0.3s;
        }

        /* 时间轴 */
        .timeline-container {
            position: absolute;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            width: 65%;
            max-width: 700px;
            background: rgba(250, 247, 242, 0.98);
            backdrop-filter: blur(10px);
            border-radius: 30px;
            padding: 10px 20px 8px 20px;
            z-index: 500;
            border: 2px solid var(--gold);
            box-shadow: 0 8px 20px rgba(0,0,0,0.15);
        }
        
        .timeline-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 3px;
        }
        
        .timeline-year-main {
            font-size: 2rem;
            font-weight: 800;
            background: linear-gradient(135deg, var(--primary), var(--primary-dark));
            -webkit-background-clip: text;
            background-clip: text;
            -webkit-text-fill-color: transparent;
            color: var(--primary);
            line-height: 1;
        }
        
        .timeline-era {
            font-size: 0.9rem;
            color: var(--gold);
            font-weight: 600;
            background: rgba(197, 160, 40, 0.15);
            padding: 3px 10px;
            border-radius: 25px;
        }
        
        .timeline-era i {
            margin-right: 4px;
            color: var(--primary);
        }
        
        .timeline-slider-main {
            width: 100%;
            margin: 5px 0 3px;
        }
        
        input[type=range] {
            width: 100%;
            height: 6px;
            -webkit-appearance: none;
            appearance: none;
            background: linear-gradient(90deg, #9d4e1f, #f4d03f, #e67e22, #c0392b);
            border-radius: 8px;
            outline: none;
        }
        
        input[type=range]::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 18px;
            height: 18px;
            background: var(--primary);
            border-radius: 50%;
            cursor: pointer;
            border: 2px solid var(--gold);
            box-shadow: 0 0 8px rgba(179,27,27,0.5);
        }
        
        input[type=range]::-moz-range-thumb {
            width: 18px;
            height: 18px;
            background: var(--primary);
            border-radius: 50%;
            cursor: pointer;
            border: 2px solid var(--gold);
        }
        
        .timeline-event {
            background: rgba(179, 27, 27, 0.05);
            border-left: 3px solid var(--primary);
            padding: 8px 12px;
            border-radius: 8px;
            margin-top: 6px;
            display: flex;
            align-items: flex-start;
            gap: 10px;
        }
        
        .event-icon {
            font-size: 1.5rem;
            min-width: 35px;
            text-align: center;
            margin-top: 2px;
        }
        
        .event-content {
            flex: 1;
        }
        
        .event-title {
            font-size: 1rem;
            font-weight: 700;
            color: var(--primary);
            margin-bottom: 3px;
            border-bottom: 1px dashed var(--gold-light);
            padding-bottom: 2px;
        }
        
        .event-desc {
            font-size: 0.8rem;
            color: var(--text-soft);
            line-height: 1.4;
            margin-bottom: 3px;
        }
        
        .event-detail {
            font-size: 0.75rem;
            color: var(--text-dark);
            background: rgba(255, 255, 255, 0.5);
            padding: 4px 8px;
            border-radius: 5px;
            margin: 3px 0;
            border-left: 2px solid var(--gold);
        }
        
        .event-context {
            font-size: 0.7rem;
            color: var(--gold);
            margin-top: 3px;
            display: flex;
            align-items: center;
            gap: 3px;
        }
        
        .event-context i {
            margin-right: 2px;
            color: var(--primary);
        }

        .share-modal {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: white;
            border-radius: 20px;
            padding: 20px;
            z-index: 2000;
            border: 3px solid var(--gold);
            display: none;
            width: 280px;
        }
        
        .share-modal.active {
            display: block;
        }
        
        .modal-overlay {
            position: fixed;
            top: 0; left: 0; right: 0; bottom: 0;
            background: rgba(0,0,0,0.5);
            z-index: 1999;
            display: none;
        }
        
        .modal-overlay.active {
            display: block;
        }

        .footer {
            text-align: center;
            padding: 10px;
            font-size: 0.8rem;
            background: white;
            border-top: 1px solid var(--gold);
        }
    </style>
</head>
<body>
    <header class="header">
        <div class="header-container">
            <h1>
                <span class="title-main"><i class="fas fa-star"></i> 杨水才精神之旅</span>
                <span class="title-sub">小车不倒只管推</span>
            </h1>
            <div class="header-right">
                <div class="share-btn" id="shareBtn">
                    <i class="fas fa-share-alt"></i> 分享
                </div>
            </div>
        </div>
    </header>

    <div class="map-stage">
        <div id="map"></div>
        
        <!-- 地图加载失败提示 -->
        <div class="map-error-message" id="mapError">
            <i class="fas fa-map" style="font-size:3rem; color:var(--primary);"></i>
            <p style="margin:10px 0;">地图加载失败，请检查Key配置</p>
        </div>

        <!-- 左侧主面板 -->
        <div class="story-panel">
            <h2>
                杨水才<br>
                <small>1924.6.29 - 1966.12.4 · 河南许昌</small>
            </h2>
            
            <div class="old-photo" id="photoViewer">
                <img src="https://revolution-multimedia.bj.bcebos.com/%E4%BA%BA%E7%89%A95.png" alt="杨水才同志肖像" id="historyPhoto">
                <div class="photo-caption" id="photoCaption">
                    1949年 · 杨水才获"人民功臣"称号
                </div>
            </div>
            
            <div class="quote-box">
                <i class="fas fa-quote-left" style="color:var(--gold);"></i>
                <p>“小车不倒只管推，只要还有一口气，就要干革命！”</p>
            </div>
            
            <div class="stats-grid">
                <div class="stat-item">
                    <div class="stat-value" id="statMileage">0</div>
                    <div class="stat-label"><i class="fas fa-route"></i> 累计里程(km)</div>
                </div>
                <div class="stat-item">
                    <div class="stat-value" id="statEarth">0</div>
                    <div class="stat-label"><i class="fas fa-mountain"></i> 累计土方(m³)</div>
                </div>
                <div class="stat-item">
                    <div class="stat-value" id="statArea">0</div>
                    <div class="stat-label"><i class="fas fa-seedling"></i> 累计灌溉(亩)</div>
                </div>
                <div class="stat-item">
                    <div class="stat-value" id="statPeople">0</div>
                    <div class="stat-label"><i class="fas fa-users"></i> 带领群众(人)</div>
                </div>
            </div>
            
            <div class="section-title">
                <i class="fas fa-history"></i> 人物生平
            </div>
            
            <div class="timeline-list">
                <div class="timeline-item">
                    <span class="timeline-year">1924</span>
                    <span class="timeline-content">出生于<span class="timeline-content strong">许昌县水道杨村</span>贫苦农家</span>
                </div>
                <div class="timeline-item">
                    <span class="timeline-year">1949</span>
                    <span class="timeline-content">北京起义，获<span class="timeline-content strong">"人民功臣"</span>称号</span>
                </div>
                <div class="timeline-item">
                    <span class="timeline-year">1951</span>
                    <span class="timeline-content"><span class="timeline-content strong">复员还乡</span>，立志改变家乡</span>
                </div>
                <div class="timeline-item">
                    <span class="timeline-year">1956</span>
                    <span class="timeline-content">加入<span class="timeline-content strong">中国共产党</span>，当选副书记</span>
                </div>
                <div class="timeline-item">
                    <span class="timeline-year">1963</span>
                    <span class="timeline-content">创办<span class="timeline-content strong">桂村农业中学</span></span>
                </div>
                <div class="timeline-item">
                    <span class="timeline-year">1964</span>
                    <span class="timeline-content">开挖<span class="timeline-content strong">"幸福塘"</span>，小车精神</span>
                </div>
                <div class="timeline-item">
                    <span class="timeline-year">1966</span>
                    <span class="timeline-content"><span class="timeline-content strong">牺牲在岗位上</span>，年仅42岁</span>
                </div>
            </div>
            
            <div class="section-title">
                <i class="fas fa-star"></i> 人物事迹
            </div>
            
            <div class="story-card">
                <i class="fas fa-fist-raised"></i> <strong>土改斗争</strong>：当众揭露地主糖衣炮弹
            </div>
            
            <div class="story-card">
                <i class="fas fa-water"></i> <strong>开挖幸福塘</strong>：累得吐血继续干，"小车坏了没有？"
            </div>
            
            <div class="story-card">
                <i class="fas fa-tree"></i> <strong>绿化荒岗</strong>：烈日下多次昏倒苗圃
            </div>
            
            <div class="story-card">
                <i class="fas fa-school"></i> <strong>创办农中</strong>：奋战40天，开荒20余亩
            </div>
            
            <div class="story-highlight">
                <i class="fas fa-gem"></i> 14次被评为"劳动模范""先进生产者"
            </div>
            
            <div class="section-title">
                <i class="fas fa-book-open"></i> 轶事典故
            </div>
            
            <div class="story-card">
                <i class="fas fa-socks"></i> <strong>拒收尼龙袜</strong>："我光一辈子脚，也不要你的袜子！"
            </div>
            
            <div class="story-card">
                <i class="fas fa-seedling"></i> <strong>下井捞核桃</strong>：下井一小时，捞出公家财产
            </div>
            
            <div class="section-title">
                <i class="fas fa-quote-right"></i> 人物评价
            </div>
            
            <div class="story-card" style="background:rgba(179,27,27,0.08);">
                <i class="fas fa-newspaper"></i> <strong>《人民日报》1969.7.13</strong>："一不怕苦、二不怕死的共产主义战士"
            </div>
            
            <div class="story-highlight">
                <i class="fas fa-flag"></i> "活着是一面旗帜，躺下是一座丰碑"
            </div>
            
            <div class="milestone-list" id="milestoneList">
                <div class="milestone-item" data-year="1949">
                    <span class="milestone-year">1949</span>
                    <span class="milestone-desc"><strong>北京起义</strong> 人民功臣</span>
                </div>
                <div class="milestone-item" data-year="1951">
                    <span class="milestone-year">1951</span>
                    <span class="milestone-desc"><strong>复员返乡</strong> 带回军功章</span>
                </div>
                <div class="milestone-item" data-year="1956">
                    <span class="milestone-year">1956</span>
                    <span class="milestone-desc"><strong>加入中共</strong> 当选副书记</span>
                </div>
                <div class="milestone-item" data-year="1964">
                    <span class="milestone-year">1964</span>
                    <span class="milestone-desc"><strong>开挖幸福塘</strong> 小车精神</span>
                </div>
                <div class="milestone-item" data-year="1966">
                    <span class="milestone-year">1966</span>
                    <span class="milestone-desc"><strong>以身殉职</strong> 年仅42岁</span>
                </div>
            </div>
            
            <div class="walking-status">
                <i class="fas fa-wheelchair"></i>
                <span>推着小车行走在许昌大地上...</span>
            </div>
        </div>

        <!-- 右侧视频面板 -->
        <div class="video-panel">
            <h4><i class="fas fa-film"></i> 央视《读书》杨水才专题</h4>
            <div class="video-container">
                <video controls preload="auto" poster="https://picsum.photos/260/140?random=200" style="width:100%; background:#000;">
                    <source src="https://revolution-multimedia.bj.bcebos.com/%5B%E8%AF%BB%E4%B9%A6%5D%E5%BC%A0%E5%B0%8F%E8%8E%89%EF%BC%9A%E3%80%8A%E5%B0%8F%E8%BD%A6%E4%B8%8D%E5%80%92%E5%8F%AA%E7%AE%A1%E6%8E%A8%E3%80%8B_CCTV%E8%8A%82%E7%9B%AE%E5%AE%98%E7%BD%91-CCTV-10_%E5%A4%AE%E8%A7%86%E7%BD%91(cctv.com)%20%E5%92%8C%E5%8F%A6%E5%A4%96%205%20%E4%B8%AA%E9%A1%B5%E9%9D%A2%20-%20%E4%B8%AA%E4%BA%BA%20-%20Microsoft%E2%80%8B%20Edge%202026-03-03%2013-20-29.mp4" type="video/mp4">
                        您的浏览器不支持视频播放
                    </source></video>
            </div>
            <div class="video-source">
                <i class="fas fa-check-circle"></i> 央视《读书》完整版
            </div>
        </div>

        <!-- 右侧数据看板 -->
        <div class="dashboard-panel">
            <h3>
                <i class="fas fa-chart-line"></i> 奋斗数据趋势
                <span style="float:right; font-size:0.8rem;" id="currentYearDisplay">1949</span>
            </h3>
            
            <div class="chart-container">
                <canvas id="mileageChart"></canvas>
            </div>
            
            <div class="data-row">
                <span class="data-label"><i class="fas fa-water"></i> 幸福塘进度</span>
                <span class="data-value" id="pondProgress">0%</span>
            </div>
            <div class="progress-bar">
                <div class="progress-fill" id="pondFill" style="width:0%"></div>
            </div>
            
            <div class="data-row">
                <span class="data-label"><i class="fas fa-tree"></i> 累计绿化</span>
                <span class="data-value" id="greenArea">0 亩</span>
            </div>
            
            <div class="data-row">
                <span class="data-label"><i class="fas fa-clock"></i> 累计工时</span>
                <span class="data-value" id="workHours">0 天</span>
            </div>
        </div>

        <!-- 时间轴 -->
        <div class="timeline-container">
            <div class="timeline-header">
                <span class="timeline-year-main" id="mainYear">1949</span>
                <span class="timeline-era" id="eraDisplay">
                    <i class="fas fa-flag"></i> 新中国成立
                </span>
            </div>
            
            <input type="range" id="mainTimeline" class="timeline-slider-main" min="1949" max="1966" value="1949" step="1">
            
            <div class="timeline-event" id="timelineEvent">
                <div class="event-icon" id="eventIcon">⭐</div>
                <div class="event-content">
                    <div class="event-title" id="eventTitle">北京随军起义</div>
                    <div class="event-desc" id="eventDesc">1949年1月，杨水才在北京随傅作义部队起义，加入解放军。</div>
                    <div class="event-detail" id="eventDetail">参加解放武汉、长沙、桂林等战役，荣获"人民功臣"称号。</div>
                    <div class="event-context" id="eventContext"><i class="fas fa-clock"></i> 时代背景：新中国成立</div>
                </div>
            </div>
        </div>
    </div>

    <!-- 分享弹窗 -->
    <div class="modal-overlay" id="modalOverlay"></div>
    <div class="share-modal" id="shareModal">
        <h3 style="font-size:1.2rem; text-align:center;">分享杨水才精神</h3>
        <div style="display:flex; justify-content:space-around; margin:20px 0;">
            <div class="share-option" onclick="shareTo('weibo')" style="width:50px;height:50px;background:#e6162d;color:white;border-radius:50%;display:flex;align-items:center;justify-content:center;cursor:pointer;"><i class="fab fa-weibo"></i></div>
            <div class="share-option" onclick="shareTo('wechat')" style="width:50px;height:50px;background:#2bae2b;color:white;border-radius:50%;display:flex;align-items:center;justify-content:center;cursor:pointer;"><i class="fab fa-weixin"></i></div>
            <div class="share-option" onclick="shareTo('qq')" style="width:50px;height:50px;background:#12b7f5;color:white;border-radius:50%;display:flex;align-items:center;justify-content:center;cursor:pointer;"><i class="fab fa-qq"></i></div>
        </div>
    </div>

    <footer class="footer">
        杨水才精神永存 · 小车不倒只管推 · 1924-1966
    </footer>

    <script>
        (function() {
            // 分享功能
            window.shareTo = function(platform) {
                alert('分享当前年份' + document.getElementById('mainYear').innerText + '的杨水才事迹');
                document.getElementById('shareModal').classList.remove('active');
                document.getElementById('modalOverlay').classList.remove('active');
            };
            
            document.getElementById('shareBtn').addEventListener('click', () => {
                document.getElementById('shareModal').classList.add('active');
                document.getElementById('modalOverlay').classList.add('active');
            });
            
            document.getElementById('modalOverlay').addEventListener('click', () => {
                document.getElementById('shareModal').classList.remove('active');
                document.getElementById('modalOverlay').classList.remove('active');
            });

            // 高德地图初始化
            function initMap() {
                // 检查高德地图是否加载成功
                if (typeof AMap === 'undefined') {
                    document.getElementById('mapError').classList.add('show');
                    document.getElementById('mapError').innerHTML += '<p>高德地图API加载失败，请检查网络</p>';
                    return;
                }
                
                try {
                    // 创建地图实例 - 平面视角（2D）- 调整中心点和缩放级别，以便同时看到北京和河南
                    const map = new AMap.Map('map', {
                        center: [115.0, 36.5],      // 中心点调整到河北山东交界，可以同时看到北京和河南
                        zoom: 7,                      // 缩放级别7可以看到华北地区
                        viewMode: '2D',
                        pitch: 0,
                        mapStyle: 'amap://styles/light',
                        showLabel: true,
                        labelzIndex: 100
                    });

                    // 添加地图控件
                    map.addControl(new AMap.Scale());
                    map.addControl(new AMap.ToolBar());

                    console.log("高德地图初始化成功");

                    // 足迹数据
                    const footprint = [
                        {
                            year: 1949,
                            lng: 116.3975,
                            lat: 39.9085,
                            title: '北京随军起义',
                            desc: '1949年1月，杨水才在北京随傅作义部队起义，加入中国人民解放军。',
                            detail: '参加解放武汉、长沙、衡阳、桂林等战役，荣获"人民功臣"称号。',
                            icon: '⭐',
                            photo: 'https://revolution-multimedia.bj.bcebos.com/%E4%BA%BA%E7%89%A95.png',
                            photoCaption: '1949年 · 杨水才获"人民功臣"'
                        },
                        {
                            year: 1951,
                            lng: 113.7983,
                            lat: 34.0542,
                            title: '复员返乡',
                            desc: '1951年10月，杨水才复员回到家乡许昌县水道杨村。',
                            detail: '谢绝组织安排，立志改变家乡。带回一本《论共产党员的修养》。',
                            icon: '🚩',
                            photo: 'https://revolution-multimedia.bj.bcebos.com/%E4%BA%BA%E7%89%A92.jpeg',
                            photoCaption: '1951年 · 杨水才复员返乡'
                        },
                        {
                            year: 1956,
                            lng: 113.7983,
                            lat: 34.0542,
                            title: '加入中国共产党',
                            desc: '1956年1月，杨水才加入中国共产党，当选党支部副书记。',
                            detail: '提出"抓水利、搞绿化、变岗地为良田"的规划。',
                            icon: '🔴',
                            photo: 'https://revolution-multimedia.bj.bcebos.com/%E4%BA%BA%E7%89%A93.jpeg',
                            photoCaption: '1956年 · 杨水才入党'
                        },
                        {
                            year: 1964,
                            lng: 113.7983,
                            lat: 34.0542,
                            title: '开挖幸福塘',
                            desc: '1964年春，杨水才带领群众开挖"幸福塘"。',
                            detail: '身患重病仍日夜奋战，留下"小车不倒只管推"名言。',
                            icon: '⛏️',
                            photo: 'https://revolution-multimedia.bj.bcebos.com/%E4%BA%BA%E7%89%A91.jpeg',
                            photoCaption: '1964年 · 杨水才在幸福塘'
                        },
                        {
                            year: 1966,
                            lng: 113.7983,
                            lat: 34.0542,
                            title: '以身殉职',
                            desc: '1966年12月4日，杨水才在办公桌前殉职，年仅42岁。',
                            detail: '桌上放着未写完的《进一步建设水道杨计划》。',
                            icon: '🕯️',
                            photo: 'https://revolution-multimedia.bj.bcebos.com/%E4%BA%BA%E7%89%A94.jpeg',
                            photoCaption: '1966年 · 最后的身影'
                        }
                    ];

                    // 创建五角星图标
                    const starIcon = new AMap.Icon({
                        image: 'data:image/svg+xml,' + encodeURIComponent('<svg width="28" height="28" viewBox="0 0 24 24"><path d="M12 2 L15 9 L22 9 L16 14 L19 22 L12 17 L5 22 L8 14 L2 9 L9 9 Z" fill="#FFD700" stroke="#b31b1b" stroke-width="2"/></svg>'),
                        size: [28, 28],
                        imageSize: [28, 28]
                    });

                    // 添加足迹点
                    footprint.forEach(item => {
                        const marker = new AMap.Marker({
                            position: [item.lng, item.lat],
                            icon: starIcon,
                            title: item.title,
                            offset: [0, -14]
                        });
                        
                        marker.setMap(map);
                        
                        // 添加点击事件
                        marker.on('click', () => {
                            const content = `
                                <div style="max-width:280px; padding:12px;">
                                    <h4 style="color:#b31b1b; margin:0 0 8px;">${item.title} (${item.year})</h4>
                                    <p style="margin:0; line-height:1.5;">${item.desc}</p>
                                    <p style="margin-top:8px;"><b>详细:</b> ${item.detail}</p>
                                </div>
                            `;
                            const infoWindow = new AMap.InfoWindow({
                                content: content,
                                offset: [0, -30]
                            });
                            infoWindow.open(map, [item.lng, item.lat]);
                        });
                    });

                    // ===== 重要地点 - 超大间距布局，包含北京起义 =====
                    const importantSites = [
                        {
                            name: '幸福塘',
                            lng: 113.6500,
                            lat: 34.0542,
                            desc: '1964年春，杨水才带领群众开挖"幸福塘"，推着独轮小车日夜奋战。历时两年挖成两个大水塘，解决数百亩旱地灌溉问题。',
                            imageUrl: 'https://revolution-multimedia.bj.bcebos.com/IMG_20260312_151137.jpg'
                        },
                        {
                            name: '水道杨村',
                            lng: 113.9000,
                            lat: 34.0542,
                            desc: '杨水才的家乡，1924年6月29日他出生于此。1951年10月复员返乡后，立志改变家乡贫穷面貌。',
                            imageUrl: 'https://revolution-multimedia.bj.bcebos.com/IMG_20260312_151756.jpg'
                        },
                        {
                            name: '桂村农业中学',
                            lng: 113.7983,
                            lat: 34.1500,
                            desc: '1963年9月1日创办，杨水才被推选为校长。1965年秋，他带领师生奋战40多天，建房21间，开垦荒地20余亩。',
                            imageUrl: 'https://revolution-multimedia.bj.bcebos.com/IMG_20260312_150635.png'
                        },
                        {
                            name: '北京起义',
                            lng: 116.3975,
                            lat: 39.9085,
                            desc: '1949年1月，杨水才在北京随傅作义部队起义，加入中国人民解放军。参加解放武汉、长沙、衡阳、桂林等战役，荣获"人民功臣"称号。',
                            imageUrl: 'https://revolution-multimedia.bj.bcebos.com/5.jpg'
                        }
                    ];

                    // 添加重要地点
                    importantSites.forEach((site, index) => {
                        // 北京起义图标可以稍大一些
                        let iconSize = 48;
                        if (site.name === '北京起义') {
                            iconSize = 56; // 北京起义图标稍大，突出重要性
                        }
                        
                        const icon = new AMap.Icon({
                            image: site.imageUrl,
                            size: [iconSize, iconSize],
                            imageSize: [iconSize, iconSize]
                        });
                        
                        const marker = new AMap.Marker({
                            position: [site.lng, site.lat],
                            icon: icon,
                            title: site.name,
                            offset: [0, -iconSize/2]
                        });
                        
                        marker.setMap(map);
                        
                        marker.on('click', () => {
                            const content = `
                                <div style="max-width:280px; padding:12px; background:white; border-radius:8px; border:2px solid #c5a028;">
                                    <h4 style="color:#b31b1b; margin:0 0 8px;">${site.name}</h4>
                                    <img src="${site.imageUrl}" style="width:100%; height:150px; object-fit:cover; border-radius:6px; margin-bottom:10px;">
                                    <p style="margin:0; line-height:1.5;">${site.desc}</p>
                                </div>
                            `;
                            const infoWindow = new AMap.InfoWindow({
                                content: content,
                                offset: [0, -30],
                                closeWhenClickMap: true
                            });
                            infoWindow.open(map, [site.lng, site.lat]);
                        });
                    });

                    // 绘制足迹连线
                    const linePoints = footprint.map(item => [item.lng, item.lat]);
                    
                    const polyline = new AMap.Polyline({
                        path: linePoints,
                        strokeColor: '#b31b1b',
                        strokeWeight: 5,
                        strokeOpacity: 0.7,
                        strokeStyle: 'dashed',
                        lineDash: [8, 5]
                    });
                    
                    polyline.setMap(map);

                    // 人物动画 - 使用传统服饰虚拟人物
                    const heroIcon = new AMap.Icon({
                        image: 'https://cdn-icons-png.flaticon.com/512/3135/3135715.png',
                        size: [48, 48],
                        imageSize: [48, 48]
                    });
                    
                    const heroMarker = new AMap.Marker({
                        position: [footprint[0].lng, footprint[0].lat],
                        icon: heroIcon,
                        offset: [0, -24]
                    });
                    
                    heroMarker.setMap(map);

                    // 动画状态
                    let currentTargetIndex = 1;
                    let currentStep = 0;
                    const stepIncrement = 0.015;

                    function startWalking() {
                        setInterval(() => {
                            if (currentTargetIndex >= footprint.length) currentTargetIndex = 1;
                            const from = footprint[currentTargetIndex-1];
                            const to = footprint[currentTargetIndex];
                            
                            const lng = from.lng + (to.lng - from.lng) * currentStep;
                            const lat = from.lat + (to.lat - from.lat) * currentStep;
                            
                            heroMarker.setPosition([lng, lat]);

                            currentStep += stepIncrement;
                            if (currentStep >= 1.0) {
                                currentStep = 0;
                                currentTargetIndex++;
                            }
                        }, 60);
                    }
                    startWalking();

                    // 图表
                    try {
                        const ctx = document.getElementById('mileageChart').getContext('2d');
                        new Chart(ctx, {
                            type: 'line',
                            data: {
                                labels: ['1949', '1951', '1956', '1964', '1966'],
                                datasets: [{
                                    data: [0, 800, 2500, 7800, 10200],
                                    borderColor: '#b31b1b',
                                    backgroundColor: 'rgba(179,27,27,0.1)',
                                    tension: 0.3,
                                    fill: true,
                                    pointBackgroundColor: '#c5a028',
                                    pointBorderColor: '#b31b1b',
                                    pointRadius: 4
                                }]
                            },
                            options: {
                                responsive: true,
                                maintainAspectRatio: false,
                                plugins: { legend: { display: false } },
                                scales: { x: { display: false }, y: { display: false } }
                            }
                        });
                    } catch (e) {
                        console.log('图表加载失败', e);
                    }

                    // 数据映射
                    const dataMap = {
                        1949: { mileage: 0, earth: 0, area: 0, people: 0, pond: 0, green: 0, hours: 0 },
                        1950: { mileage: 300, earth: 80, area: 12, people: 6, pond: 2, green: 8, hours: 45 },
                        1951: { mileage: 800, earth: 200, area: 30, people: 15, pond: 5, green: 20, hours: 120 },
                        1952: { mileage: 1300, earth: 380, area: 52, people: 23, pond: 9, green: 34, hours: 190 },
                        1953: { mileage: 1800, earth: 550, area: 78, people: 30, pond: 13, green: 48, hours: 265 },
                        1954: { mileage: 2200, earth: 750, area: 105, people: 38, pond: 17, green: 62, hours: 345 },
                        1955: { mileage: 2350, earth: 1000, area: 128, people: 42, pond: 19, green: 72, hours: 400 },
                        1956: { mileage: 2500, earth: 1200, area: 150, people: 45, pond: 20, green: 80, hours: 450 },
                        1957: { mileage: 2900, earth: 1450, area: 175, people: 51, pond: 24, green: 96, hours: 510 },
                        1958: { mileage: 3400, earth: 1750, area: 210, people: 59, pond: 28, green: 120, hours: 600 },
                        1959: { mileage: 3900, earth: 2100, area: 255, people: 68, pond: 33, green: 150, hours: 710 },
                        1960: { mileage: 4400, earth: 2500, area: 310, people: 80, pond: 39, green: 180, hours: 830 },
                        1961: { mileage: 4800, earth: 2900, area: 370, people: 92, pond: 45, green: 215, hours: 950 },
                        1962: { mileage: 5300, earth: 3250, area: 425, people: 103, pond: 51, green: 245, hours: 1060 },
                        1963: { mileage: 5800, earth: 3550, area: 475, people: 112, pond: 58, green: 270, hours: 1170 },
                        1964: { mileage: 6200, earth: 3800, area: 520, people: 120, pond: 65, green: 280, hours: 1260 },
                        1965: { mileage: 8200, earth: 4700, area: 685, people: 160, pond: 82, green: 365, hours: 1680 },
                        1966: { mileage: 10200, earth: 5600, area: 850, people: 200, pond: 100, green: 450, hours: 2100 }
                    };

                    // 时间轴元素
                    const slider = document.getElementById('mainTimeline');
                    const yearSpan = document.getElementById('mainYear');
                    const eraDisplay = document.getElementById('eraDisplay');
                    const eventIcon = document.getElementById('eventIcon');
                    const eventTitle = document.getElementById('eventTitle');
                    const eventDesc = document.getElementById('eventDesc');
                    const eventDetail = document.getElementById('eventDetail');
                    const eventContext = document.getElementById('eventContext');
                    
                    function updateDisplay(year) {
                        year = parseInt(year);
                        
                        yearSpan.innerText = year;
                        document.getElementById('currentYearDisplay').innerText = year;
                        
                        const d = dataMap[year] || dataMap[1949];
                        
                        document.getElementById('statMileage').innerText = d.mileage;
                        document.getElementById('statEarth').innerText = d.earth;
                        document.getElementById('statArea').innerText = d.area;
                        document.getElementById('statPeople').innerText = d.people;
                        document.getElementById('pondProgress').innerText = d.pond + '%';
                        document.getElementById('pondFill').style.width = d.pond + '%';
                        document.getElementById('greenArea').innerText = d.green + ' 亩';
                        document.getElementById('workHours').innerText = d.hours + ' 天';
                        
                        // 更新时代背景
                        const eraMap = {
                            1949: '新中国成立', 1950: '建国初期', 1951: '土地改革',
                            1952: '国民经济恢复', 1953: '一五计划', 1954: '第一部宪法',
                            1955: '农业合作化', 1956: '农业合作化', 1957: '一五计划',
                            1958: '大跃进', 1959: '建国十周年', 1960: '困难时期',
                            1961: '调整巩固', 1962: '七千人大会', 1963: '学雷锋',
                            1964: '农业学大寨', 1965: '农业学大寨', 1966: '文革前夕'
                        };
                        eraDisplay.innerHTML = `<i class="fas fa-flag"></i> ${eraMap[year] || '建设时期'}`;
                        
                        // 更新事件
                        const event = footprint.find(f => f.year === year);
                        if (event) {
                            eventIcon.innerText = event.icon;
                            eventTitle.innerText = event.title;
                            eventDesc.innerText = event.desc;
                            eventDetail.innerText = event.detail;
                            eventContext.innerHTML = `<i class="fas fa-clock"></i> 时代背景：${eraMap[year] || '建设时期'}`;
                            
                            document.getElementById('historyPhoto').src = event.photo;
                            document.getElementById('photoCaption').innerHTML = event.photoCaption;
                        } else {
                            eventIcon.innerText = '📅';
                            eventTitle.innerText = year + '年 · 持续奋斗';
                            eventDesc.innerText = '杨水才带领群众继续建设家乡。';
                            eventDetail.innerText = '他坚持学习毛主席著作，带领群众战天斗地。';
                            eventContext.innerHTML = `<i class="fas fa-clock"></i> 时代背景：${eraMap[year] || '建设时期'}`;
                            
                            let nearestYear = 1949;
                            for (let i=0; i<footprint.length; i++) {
                                if (footprint[i].year <= year) nearestYear = footprint[i].year;
                            }
                            const nearestEvent = footprint.find(f => f.year === nearestYear) || footprint[0];
                            document.getElementById('historyPhoto').src = nearestEvent.photo;
                            document.getElementById('photoCaption').innerHTML = nearestEvent.photoCaption;
                        }
                        
                        // 高亮年份按钮
                        document.querySelectorAll('.milestone-item').forEach(item => {
                            if (parseInt(item.dataset.year) === year) {
                                item.style.background = 'rgba(197,160,40,0.2)';
                            } else {
                                item.style.background = 'transparent';
                            }
                        });
                        
                        // 移动人物到对应位置
                        const target = footprint.find(f => f.year === year);
                        if (target) {
                            heroMarker.setPosition([target.lng, target.lat]);
                        }
                    }

                    slider.addEventListener('input', (e) => {
                        updateDisplay(e.target.value);
                        
                        const year = parseInt(e.target.value);
                        let nearestIdx = 0;
                        for (let i=0; i<footprint.length; i++) {
                            if (footprint[i].year <= year) nearestIdx = i;
                        }
                        heroMarker.setPosition([footprint[nearestIdx].lng, footprint[nearestIdx].lat]);
                    });
                    
                    document.querySelectorAll('.milestone-item').forEach(item => {
                        item.addEventListener('click', () => {
                            const year = parseInt(item.dataset.year);
                            slider.value = year;
                            updateDisplay(year);
                            
                            const idx = footprint.findIndex(f => f.year === year);
                            if (idx >= 0) {
                                heroMarker.setPosition([footprint[idx].lng, footprint[idx].lat]);
                            }
                        });
                    });

                    updateDisplay(1949);
                    
                } catch (e) {
                    console.error('地图初始化错误', e);
                    document.getElementById('mapError').classList.add('show');
                    document.getElementById('mapError').innerHTML += '<p>' + e.message + '</p>';
                }
            }

            // 等待高德地图加载
            if (typeof AMap !== 'undefined') {
                initMap();
            } else {
                window.onload = function() {
                    setTimeout(() => {
                        if (typeof AMap !== 'undefined') {
                            initMap();
                        } else {
                            document.getElementById('mapError').classList.add('show');
                        }
                    }, 1000);
                };
            }
        })();
    </script>
</body>
</html>
