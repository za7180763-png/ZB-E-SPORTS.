,<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>NB E-SPORTS</title>
    
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;500;600;700;800;900&family=Hind+Siliguri:wght@400;500;600;700&display=swap" rel="stylesheet">
    <!-- Font Awesome -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <!-- SweetAlert2 -->
    <link href="https://cdn.jsdelivr.net/npm/@sweetalert2/theme-material-ui/material-ui.min.css" rel="stylesheet">
    
    <style>
        /* ========================================
           ROOT VARIABLES — Light Theme (Screenshot Style)
           ======================================== */
        :root {
            --bg-body: #e3ebfa; /* হালকা নীল ব্যাকগ্রাউন্ড */
            --bg-card: #ffffff; /* সাদা কার্ড */
            --text-dark: #111827;
            --text-gray: #6b7280;
            --primary: #0f766e; /* ডার্ক টিল/গ্রিন (বাটন কালার) */
            --primary-light: #ccfbf1;
            --accent-purple: #7c3aed;
            --accent-red: #ef4444;
            --border-color: #e5e7eb;
            --radius-card: 20px;
            --radius-sm: 12px;
            --shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
            --transition: all 0.3s ease;
        }

        /* ========================================
           RESET & BASE
           ======================================== */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            font-family: 'Outfit', 'Hind Siliguri', sans-serif;
            background-color: var(--bg-body);
            color: var(--text-dark);
            min-height: 100vh;
            padding: 0 16px 80px 16px;
            overflow-x: hidden;
        }

        /* ========================================
           ANIMATIONS
           ======================================== */
        @keyframes fadeUp {
            from { opacity: 0; transform: translateY(15px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .animate-fade-up { opacity: 0; animation: fadeUp 0.4s ease forwards; }
        .delay-1 { animation-delay: 0.05s; }

        /* ========================================
           SPLASH SCREEN
           ======================================== */
        #app-splash {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: #fff; z-index: 10000;
            display: flex; flex-direction: column; justify-content: center; align-items: center;
            transition: opacity 0.5s ease-out;
        }
        .splash-logo-box {
            width: 100px; height: 100px;
            background: var(--primary); border-radius: 50%;
            display: flex; align-items: center; justify-content: center; padding: 10px;
            margin-bottom: 20px;
        }
        .splash-logo-box img { width: 100%; height: 100%; object-fit: contain; border-radius: 50%; }
        .splash-app-name { font-size: 24px; font-weight: 800; color: var(--text-dark); }
        .splash-sub { font-size: 12px; color: var(--text-gray); letter-spacing: 2px; text-transform: uppercase; margin-bottom: 40px; }
        .splash-loader { width: 150px; height: 4px; background: #e5e7eb; border-radius: 10px; overflow: hidden; }
        .splash-loader-fill { height: 100%; width: 0%; background: var(--primary); border-radius: 10px; transition: width 0.15s linear; }

        /* ========================================
           GLOBAL LOADER
           ======================================== */
        #global-loader {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(255,255,255,0.8); z-index: 9999;
            display: none; justify-content: center; align-items: center; flex-direction: column; gap: 16px;
        }
        #global-loader.active { display: flex; }
        .spinner { width: 40px; height: 40px; border: 4px solid #e5e7eb; border-top: 4px solid var(--primary); border-radius: 50%; animation: spin 0.8s linear infinite; }
        @keyframes spin { 100% { transform: rotate(360deg); } }
        .loader-text { font-size: 14px; color: var(--text-gray); font-weight: 600; }

        /* ========================================
           APP HEADER
           ======================================== */
        .app-header {
            display: flex; align-items: center; justify-content: space-between;
            padding: 16px 0 20px 0;
        }
        .brand-section { display: flex; align-items: center; gap: 12px; }
        .brand-logo {
            width: 40px; height: 40px; border-radius: 50%;
            background: #000; display: flex; align-items: center; justify-content: center;
            font-size: 18px; font-weight: 900; color: #fff; overflow: hidden;
        }
        .brand-name { font-size: 20px; font-weight: 800; letter-spacing: 0.3px; color: var(--text-dark); }
        .header-actions { display: flex; align-items: center; gap: 12px; }
        .header-btn {
            width: 40px; height: 40px; border-radius: 50%;
            background: transparent; border: none; color: var(--text-dark);
            display: flex; align-items: center; justify-content: center;
            font-size: 20px; cursor: pointer;
        }
        .header-btn .badge-dot { position: absolute; top: 8px; right: 8px; width: 8px; height: 8px; border-radius: 50%; background: var(--accent-red); }
        body.sub-page .app-header { display: none; }

        /* ========================================
           NOTICE BAR
           ======================================== */
        .notice-bar {
            background: var(--bg-card); border-radius: var(--radius-sm);
            padding: 10px 14px; margin-bottom: 20px;
            display: flex; align-items: center; gap: 12px;
            box-shadow: var(--shadow); overflow: hidden;
        }
        .notice-bar .pill {
            background: var(--primary-light); color: var(--primary);
            padding: 4px 14px; border-radius: 30px; font-size: 10px; font-weight: 700;
            white-space: nowrap; text-transform: uppercase;
        }
        .notice-bar .marquee { flex: 1; overflow: hidden; white-space: nowrap; }
        .notice-bar .marquee span {
            display: inline-block; animation: marqueeScroll 20s linear infinite;
            font-size: 13px; font-weight: 500; color: var(--text-dark);
        }
        @keyframes marqueeScroll { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }

        /* ========================================
           AUTH VIEW
           ======================================== */
        #auth-view {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: var(--bg-body); z-index: 5000;
            display: none; flex-direction: column; overflow-y: auto; padding: 40px 20px;
        }
        #auth-view.active { display: flex; }
        .auth-container { max-width: 400px; margin: auto; width: 100%; }
        .auth-title { font-size: 28px; font-weight: 800; text-align: center; color: var(--text-dark); margin-bottom: 8px;}
        .auth-subtitle { font-size: 14px; color: var(--text-gray); text-align: center; margin-bottom: 30px;}
        .auth-card { background: var(--bg-card); border-radius: var(--radius-card); padding: 24px 20px; box-shadow: var(--shadow); }
        
        .auth-tabs { display: flex; gap: 10px; margin-bottom: 24px; background: #f3f4f6; border-radius: var(--radius-sm); padding: 4px; }
        .auth-tab { flex: 1; padding: 12px; text-align: center; font-size: 14px; font-weight: 700; color: var(--text-gray); border: none; background: none; border-radius: 10px; cursor: pointer; transition: var(--transition); }
        .auth-tab.active { background: #fff; color: var(--text-dark); box-shadow: 0 2px 4px rgba(0,0,0,0.05); }

        .auth-form { display: none; }
        .auth-form.active { display: block; animation: fadeUp 0.4s ease forwards; }

        .form-group { margin-bottom: 16px; }
        .form-group label { display: block; font-size: 12px; font-weight: 600; color: var(--text-gray); margin-bottom: 6px; }
        .form-group input { width: 100%; padding: 14px 16px; background: #f9fafb; border: 1px solid var(--border-color); border-radius: var(--radius-sm); color: var(--text-dark); font-size: 14px; outline: none; }
        .form-group input:focus { border-color: var(--primary); background: #fff; }
        .auth-checkbox-row { display: flex; align-items: center; gap: 10px; margin: 10px 0 20px; font-size: 12px; color: var(--text-gray); }
        .btn-auth { width: 100%; padding: 16px; background: var(--primary); border: none; border-radius: var(--radius-sm); color: #fff; font-size: 16px; font-weight: 700; cursor: pointer; }
        
        /* ========================================
           MAIN CONTENT
           ======================================== */
        #main-content { display: none; }
        #main-content.show { display: block; }
        .app-section { display: none; animation: fadeUp 0.4s ease forwards; }
        .app-section.active { display: block; }

        /* ========================================
           HOME - SLIDER & CATEGORIES
           ======================================== */
        .slider-container { width: 100%; aspect-ratio: 2.1/1; border-radius: var(--radius-card); overflow: hidden; position: relative; background: var(--bg-card); margin-bottom: 0; box-shadow: var(--shadow); }
        .slider-wrapper { display: flex; width: 100%; height: 100%; transition: transform 0.6s ease; }
        .slide { min-width: 100%; height: 100%; }
        .slide img { width: 100%; height: 100%; object-fit: cover; display: block; }
        .slider-dots { display: flex; justify-content: center; gap: 6px; padding: 12px 0; }
        .dot { width: 6px; height: 6px; background: #cbd5e1; border-radius: 50%; }
        .dot.active { width: 20px; background: var(--primary); border-radius: 10px; }

        .section-header-custom { display: flex; justify-content: space-between; align-items: center; margin: 20px 0 14px 0; }
        .section-title-custom { font-size: 16px; font-weight: 600; color: var(--text-dark); }
        
        .category-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 14px; }
        .cat-card { background: var(--bg-card); border-radius: var(--radius-card); overflow: hidden; padding: 12px; box-shadow: var(--shadow); cursor: pointer; }
        .cat-card-img { width: 100%; aspect-ratio: 16/10; object-fit: cover; border-radius: 12px; }
        .cat-card-info { padding-top: 10px; }
        .cat-card-title { font-weight: 700; font-size: 15px; color: var(--text-dark); }
        .cat-card-count { font-size: 12px; color: var(--text-gray); font-weight: 500; margin-top: 2px; }

        /* ========================================
           PROFILE HEADER & MENU
           ======================================== */
        .profile-header-bg { padding: 20px 0; text-align: center; margin-bottom: 10px; }
        .profile-avatar-box { width: 70px; height: 70px; border-radius: 50%; margin: 0 auto 10px; overflow: hidden; background: #fff; box-shadow: var(--shadow); }
        .profile-avatar-box img { width: 100%; height: 100%; object-fit: cover; }
        .profile-name { font-size: 18px; font-weight: 800; color: var(--text-dark); }
        .profile-email { font-size: 13px; color: var(--text-gray); margin-bottom: 15px;}
        
        .profile-quick-actions { display: flex; justify-content: space-around; padding: 12px; background: #fff; border-radius: 20px; margin-bottom: 20px; box-shadow: var(--shadow); }
        .p-qa-btn { display: flex; flex-direction: column; align-items: center; gap: 6px; background: none; border: none; color: var(--text-gray); font-size: 11px; font-weight: 600; cursor: pointer; }
        .p-qa-btn i { font-size: 18px; color: var(--text-dark); }

        .menu-list { background: var(--bg-card); border-radius: var(--radius-card); padding: 8px 16px; box-shadow: var(--shadow); }
        .menu-item { display: flex; align-items: center; padding: 16px 0; border-bottom: 1px solid var(--border-color); gap: 14px; color: var(--text-dark); text-decoration: none; background: none; width: 100%; font-size: 14px; font-weight: 600; cursor: pointer; border-left: none; border-right: none; border-top: none;}
        .menu-item:last-child { border-bottom: none; }
        .menu-item .icon-wrap i { font-size: 18px; color: var(--text-dark); }
        .menu-item .menu-text { flex: 1; text-align: left; }
        .menu-item .chevron { color: #9ca3af; font-size: 12px; }

        /* ========================================
           MATCH CARDS (Screenshot Style)
           ======================================== */
        .match-card { background: var(--bg-card); border-radius: var(--radius-card); overflow: hidden; margin-bottom: 20px; padding: 0; box-shadow: var(--shadow); cursor: pointer; }
        .banner-box { position: relative; width: 100%; height: 180px; }
        .banner-box img { width: 100%; height: 100%; object-fit: cover; }
        .banner-overlay { position: absolute; inset: 0; background: linear-gradient(to top, rgba(0,0,0,0.85) 0%, transparent 60%); }
        .badge-map, .badge-id { position: absolute; font-size: 10px; font-weight: 700; padding: 4px 10px; border-radius: 20px; color: #fff; }
        .badge-map { top: 12px; left: 12px; background: #10b981; }
        .badge-id { top: 12px; right: 12px; background: rgba(0,0,0,0.6); }
        .timer-badge { position: absolute; top: 45px; left: 12px; background: rgba(255,255,255,0.2); backdrop-filter: blur(4px); color: #fff; font-size: 10px; font-weight: 600; padding: 4px 10px; border-radius: 20px; }
        .banner-title { position: absolute; bottom: 12px; left: 12px; font-size: 20px; font-weight: 800; color: #fff; text-shadow: 0 2px 4px rgba(0,0,0,0.5); }

        .stats-grid { display: grid; grid-template-columns: repeat(4, 1fr); margin: 16px 16px 10px 16px; padding: 12px; background: #f8fafc; border-radius: var(--radius-sm); text-align: center; }
        .stat-item { display: flex; flex-direction: column; align-items: center; }
        .stat-item i { font-size: 14px; margin-bottom: 6px; }
        .stat-item:nth-child(1) i { color: #d946ef; }
        .stat-item:nth-child(2) i { color: #ef4444; }
        .stat-item:nth-child(3) i { color: #f59e0b; }
        .stat-item:nth-child(4) i { color: #10b981; }
        .stat-lbl { font-size: 10px; color: var(--text-gray); margin-bottom: 2px; font-weight: 600; }
        .stat-val { font-size: 13px; font-weight: 800; color: var(--text-dark); }

        .action-container { display: flex; flex-direction: column; gap: 10px; padding: 0 16px 16px 16px; margin-top: 5px;}
        .spots-left { font-size: 11px; color: #ef4444; font-weight: 600; text-align: right; margin-bottom: -5px;}
        .bar-bg { height: 8px; background: #e5e7eb; border-radius: 10px; overflow: hidden; }
        .bar-fill { height: 100%; background: #0f766e; border-radius: 10px; width: 0; }

        .btn-join-main { background: #0f766e; color: #fff; padding: 12px 24px; border: none; border-radius: 20px; font-weight: 600; font-size: 14px; cursor: pointer; width: 100%; }
        .btn-join-main.disabled { background: #9ca3af; }
        .btn-join-main.result-btn { background: #e2e8f0; color: var(--text-dark); }
        
        .bottom-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; padding: 0 16px 16px 16px; }
        .btn-bottom { background: #f8fafc; border: 1px solid var(--border-color); padding: 10px; border-radius: 12px; font-size: 12px; font-weight: 600; color: var(--text-gray); display: flex; align-items: center; justify-content: center; gap: 6px; }
        .btn-bottom:first-child { background: #fff; border-color: #e2e8f0; color: var(--text-dark); }

        /* ========================================
           MATCH TABS
           ======================================== */
        .match-tabs-container { display: flex; gap: 10px; margin: 0 0 16px 0; }
        .m-tab { flex: 1; padding: 12px; text-align: center; font-size: 14px; font-weight: 600; color: var(--text-gray); border: none; background: #e2e8f0; border-radius: 20px; cursor: pointer; }
        .m-tab.active { background: #0f766e; color: #fff; }

        /* ========================================
           WALLET & SUB PAGES (Light Theme)
           ======================================== */
        .wallet-gradient-card { background: #0f766e; border-radius: var(--radius-card); padding: 24px; color: #fff; margin-bottom: 20px; box-shadow: var(--shadow); }
        .wg-label { font-size: 12px; opacity: 0.8; }
        .wg-balance { font-size: 32px; font-weight: 800; margin-top: 5px; }
        .wallet-stats { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-bottom: 20px; }
        .wallet-stat { background: #fff; border-radius: 16px; padding: 16px; text-align: center; box-shadow: var(--shadow); }
        .ws-amount { font-size: 18px; font-weight: 800; color: var(--text-dark); margin-top: 4px;}
        .ws-label { font-size: 10px; color: var(--text-gray); font-weight: 700; text-transform: uppercase; }

        .wallet-actions { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-bottom: 20px; }
        .wa-btn { padding: 14px; border: none; border-radius: 16px; font-weight: 700; font-size: 14px; cursor: pointer; display: flex; align-items: center; justify-content: center; gap: 8px; }
        .wa-btn.add { background: #0f766e; color: #fff; }
        .wa-btn.withdraw { background: #10b981; color: #fff; }

        .method-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; margin-bottom: 20px; }
        .method-card { background: #fff; border: 2px solid var(--border-color); border-radius: 12px; padding: 14px 8px; text-align: center; cursor: pointer; }
        .method-card.active { border-color: #0f766e; background: #f0fdfa; }
        .method-card img { width: 32px; height: 32px; object-fit: contain; margin: 0 auto 4px; display: block; }
        .method-card span { font-size: 11px; font-weight: 600; color: var(--text-gray); }

        .am-box { background: #fff; border-radius: var(--radius-card); padding: 20px; margin-bottom: 16px; box-shadow: var(--shadow); }
        .am-input { width: 100%; padding: 14px 16px; background: #f9fafb; border: 1px solid var(--border-color); border-radius: 12px; color: var(--text-dark); font-size: 14px; outline: none; margin-bottom: 16px; }
        .am-verify-btn { width: 100%; padding: 16px; background: #0f766e; color: #fff; border: none; border-radius: 16px; font-size: 16px; font-weight: 700; cursor: pointer; }
        
        .wd-input-group { display: flex; align-items: center; gap: 12px; background: #f9fafb; border: 1px solid var(--border-color); border-radius: 12px; padding: 4px 14px; margin-bottom: 14px; }
        .wd-input-group input { flex: 1; padding: 14px 0; background: transparent; border: none; color: var(--text-dark); font-size: 14px; outline: none; }
        .wd-submit-btn { width: 100%; padding: 16px; background: #10b981; color: #fff; border: none; border-radius: 16px; font-size: 16px; font-weight: 700; cursor: pointer; }

        /* History / Empty msg */
        .history-card { background: #fff; border-radius: 12px; padding: 14px 16px; margin-bottom: 10px; display: flex; justify-content: space-between; align-items: center; box-shadow: var(--shadow); border-left: 4px solid var(--primary); }
        .hc-title { font-weight: 700; font-size: 14px; color: var(--text-dark); }
        .hc-date { font-size: 11px; color: var(--text-gray); }
        .hc-amount { font-size: 13px; f
