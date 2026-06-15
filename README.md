<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Pendleton Local | Discover The West Downtown</title>
    
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin=""/>
    
    <style>
        :root {
            --cream: #F5F1E8;
            --charcoal: #2C2C2C;
            --leather: #6B4F3A;
            --sage: #9CAF88;
            --rust: #B7410E;
            --sand: #E8DCC6;
            --white: #FFFFFF;
            --cat-food: #B7410E;
            --cat-retail: #6B4F3A;
            --cat-art: #C9A66B;
            --cat-entertainment: #9CAF88;
            --cat-coffee: #8B5A2B;
            --cat-icecream: #E8B4B8;
            --shadow: rgba(44, 44, 44, 0.08);
            --shadow-md: rgba(44, 44, 44, 0.15);
            --shadow-lg: rgba(44, 44, 44, 0.25);
            --grain: url("data:image/svg+xml,%3Csvg width='100' height='100' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.8' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100' height='100' filter='url(%23noise)' opacity='0.025'/%3E%3C/svg%3E");
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
            -webkit-font-smoothing: antialiased;
        }

        body {
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
            background: var(--cream);
            color: var(--charcoal);
            overflow: hidden;
            position: fixed;
            width: 100%;
            height: 100vh;
            height: 100dvh;
            background-image: var(--grain);
        }

        #map {
            width: 100%;
            height: 100vh;
            height: 100dvh;
            z-index: 1;
        }

        .leaflet-control-attribution {
            font-size: 10px;
            background: rgba(245, 241, 232, 0.9) !important;
            backdrop-filter: blur(8px);
            -webkit-backdrop-filter: blur(8px);
        }

        .leaflet-control-zoom {
            border: none !important;
            margin-right: 20px !important;
            margin-bottom: 90px !important;
        }

        .leaflet-control-zoom a {
            width: 44px !important;
            height: 44px !important;
            line-height: 44px !important;
            background: var(--white) !important;
            border: 2px solid var(--sand) !important;
            color: var(--leather) !important;
            font-weight: 600;
            box-shadow: 0 2px 8px var(--shadow);
        }

        .leaflet-control-zoom a:first-child {
            border-radius: 12px 12px 0 0 !important;
        }

        .leaflet-control-zoom a:last-child {
            border-radius: 0 0 12px 12px !important;
        }

        .leaflet-control-zoom a:active {
            background: var(--cream) !important;
        }

        .custom-marker {
            width: 44px;
            height: 44px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 20px;
            border: 3px solid var(--white);
            box-shadow: 0 4px 12px var(--shadow-md);
            transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
            cursor: pointer;
        }

        .custom-marker:active {
            transform: scale(0.9);
        }

        .custom-marker.active {
            transform: scale(1.15);
            box-shadow: 0 6px 20px var(--shadow-lg);
            z-index: 1000 !important;
        }

        .custom-marker.food { background: var(--cat-food); }
        .custom-marker.retail { background: var(--cat-retail); }
        .custom-marker.art { background: var(--cat-art); }
        .custom-marker.entertainment { background: var(--cat-entertainment); }
        .custom-marker.coffee { background: var(--cat-coffee); }
        .custom-marker.icecream { background: var(--cat-icecream); }

        .user-marker {
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background: #2196F3;
            border: 3px solid var(--white);
            box-shadow: 0 0 0 4px rgba(33, 150, 243, 0.3), 0 2px 8px var(--shadow-md);
            animation: pulse 2s ease-out infinite;
        }

        @keyframes pulse {
            0%, 100% { box-shadow: 0 0 0 4px rgba(33, 150, 243, 0.3), 0 2px 8px var(--shadow-md); }
            50% { box-shadow: 0 0 0 8px rgba(33, 150, 243, 0.15), 0 2px 8px var(--shadow-md); }
        }

        .sticky-header {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            z-index: 1000;
            background: rgba(245, 241, 232, 0.96);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            box-shadow: 0 1px 0 var(--sand), 0 4px 20px var(--shadow);
        }

        .header-content {
            padding: 12px 16px 8px;
        }

        .brand-row {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 12px;
        }

        .brand {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .brand-icon {
            width: 36px;
            height: 36px;
            background: linear-gradient(135deg, var(--leather) 0%, var(--rust) 100%);
            border-radius: 9px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 18px;
            box-shadow: 0 2px 8px var(--shadow-md);
        }

        .brand-text h1 {
            font-size: 17px;
            font-weight: 700;
            color: var(--leather);
            letter-spacing: -0.4px;
            line-height: 1;
        }

        .brand-text p {
            font-size: 11px;
            color: var(--charcoal);
            opacity: 0.6;
            margin-top: 2px;
            font-weight: 500;
        }

        .search-container {
            position: relative;
            margin-bottom: 12px;
        }

        .search-input {
            width: 100%;
            height: 48px;
            padding: 0 50px 0 16px;
            border: 2px solid var(--sand);
            border-radius: 14px;
            background: var(--white);
            font-size: 15px;
            font-family: 'Inter', sans-serif;
            color: var(--charcoal);
            transition: all 0.25s ease;
            font-weight: 500;
        }

        .search-input::placeholder {
            color: var(--charcoal);
            opacity: 0.4;
        }

        .search-input:focus {
            outline: none;
            border-color: var(--sage);
            box-shadow: 0 0 0 4px rgba(156, 175, 136, 0.12);
        }

        .search-clear {
            position: absolute;
            right: 12px;
            top: 50%;
            transform: translateY(-50%);
            width: 28px;
            height: 28px;
            border: none;
            background: var(--cream);
            border-radius: 8px;
            display: none;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            font-size: 18px;
            color: var(--leather);
            line-height: 1;
        }

        .search-clear.show {
            display: flex;
        }

        .search-clear:active {
            transform: translateY(-50%) scale(0.92);
            background: var(--sand);
        }

        .search-icon {
            position: absolute;
            right: 16px;
            top: 50%;
            transform: translateY(-50%);
            font-size: 18px;
            pointer-events: none;
            opacity: 0.5;
        }

        .filter-scroll {
            display: flex;
            gap: 8px;
            overflow-x: auto;
            padding: 0 16px 12px;
            scrollbar-width: none;
            -webkit-overflow-scrolling: touch;
        }

        .filter-scroll::-webkit-scrollbar {
            display: none;
        }

        .filter-chip {
            flex-shrink: 0;
            height: 48px;
            min-width: 48px;
            padding: 0 18px;
            border: 2px solid var(--sand);
            background: var(--white);
            border-radius: 24px;
            font-size: 14px;
            font-weight: 600;
            color: var(--charcoal);
            cursor: pointer;
            transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
            display: flex;
            align-items: center;
            gap: 7px;
            font-family: 'Inter', sans-serif;
            box-shadow: 0 1px 3px var(--shadow);
        }

        .filter-chip:active {
            transform: scale(0.94);
        }

        .filter-chip.active {
            background: var(--leather);
            border-color: var(--leather);
            color: var(--white);
            box-shadow: 0 4px 12px rgba(107, 79, 58, 0.25);
        }

        .filter-chip.active.food { background: var(--cat-food); border-color: var(--cat-food); }
        .filter-chip.active.retail { background: var(--cat-retail); border-color: var(--cat-retail); }
        .filter-chip.active.art { background: var(--cat-art); border-color: var(--cat-art); color: var(--charcoal); }
        .filter-chip.active.entertainment { background: var(--cat-entertainment); border-color: var(--cat-entertainment); }
        .filter-chip.active.coffee { background: var(--cat-coffee); border-color: var(--cat-coffee); }
        .filter-chip.active.icecream { background: var(--cat-icecream); border-color: var(--cat-icecream); color: var(--charcoal); }

        .fab-near-me {
            position: fixed;
            bottom: 24px;
            right: 20px;
            z-index: 900;
            width: 56px;
            height: 56px;
            border: none;
            background: var(--rust);
            border-radius: 28px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            box-shadow: 0 6px 20px rgba(183, 65, 14, 0.35);
            transition: all 0.25s ease;
            font-size: 24px;
            color: var(--white);
        }

        .fab-near-me:active {
            transform: scale(0.92);
            box-shadow: 0 4px 12px rgba(183, 65, 14, 0.3);
        }

        .fab-near-me:disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }

        .fab-near-me.loading {
            animation: spin 0.8s linear infinite;
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        .bottom-sheet {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            z-index: 2000;
            background: var(--white);
            border-radius: 28px 28px 0 0;
            box-shadow: 0 -10px 40px var(--shadow-lg);
            transform: translateY(100%);
            transition: transform 0.4s cubic-bezier(0.32, 0.72, 0, 1);
            max-height: 90vh;
            max-height: 90dvh;
            display: flex;
            flex-direction: column;
        }

        .bottom-sheet.show {
            transform: translateY(0);
        }

        .sheet-header {
            position: relative;
            padding: 12px 20px 0;
            flex-shrink: 0;
        }

        .sheet-handle {
            width: 36px;
            height: 4px;
            background: var(--sand);
            border-radius: 2px;
            margin: 0 auto 16px;
        }

        .sheet-close {
            position: absolute;
            top: 8px;
            right: 12px;
            width: 48px;
            height: 48px;
            border: none;
            background: var(--cream);
            border-radius: 14px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.2s ease;
            color: var(--leather);
            font-size: 26px;
            font-weight: 300;
            line-height: 1;
        }

        .sheet-close:active {
            transform: scale(0.92);
            background: var(--sand);
        }

        .sheet-content {
            overflow-y: auto;
            padding: 0 20px 28px;
            -webkit-overflow-scrolling: touch;
        }

        .video-wrapper {
            width: 100%;
            aspect-ratio: 16/9;
            border-radius: 18px;
            overflow: hidden;
            background: var(--charcoal);
            margin-bottom: 8px;
            box-shadow: 0 6px 20px var(--shadow-md);
            position: relative;
        }

        .video-wrapper iframe {
            width: 100%;
            height: 100%;
            border: none;
            display: block;
        }

        .video-note {
            font-size: 12px;
            color: var(--charcoal);
            opacity: 0.6;
            text-align: center;
            margin-bottom: 20px;
            font-weight: 500;
        }

        .biz-header {
            margin-bottom: 20px;
        }

        .category-badge {
            display: inline-flex;
            align-items: center;
            gap: 6px;
            padding: 7px 14px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 0.6px;
            margin-bottom: 12px;
        }

        .category-badge.food { background: rgba(183, 65, 14, 0.12); color: var(--cat-food); }
        .category-badge.retail { background: rgba(107, 79, 58, 0.12); color: var(--cat-retail); }
        .category-badge.art { background: rgba(201, 166, 107, 0.2); color: #8B7355; }
        .category-badge.entertainment { background: rgba(156, 175, 136, 0.2); color: #5a6b4a; }
        .category-badge.coffee { background: rgba(139, 90, 43, 0.15); color: var(--cat-coffee); }
        .category-badge.icecream { background: rgba(232, 180, 184, 0.3); color: #A0525A; }

        .biz-name {
            font-size: 28px;
            font-weight: 700;
            color: var(--charcoal);
            margin-bottom: 10px;
            line-height: 1.15;
            letter-spacing: -0.5px;
        }

        .biz-description {
            font-size: 15px;
            line-height: 1.65;
            color: var(--charcoal);
            opacity: 0.75;
        }

        .info-grid {
            display: flex;
            flex-direction: column;
            gap: 18px;
            margin: 24px 0;
        }

        .info-row {
            display: flex;
            align-items: flex-start;
            gap: 14px;
        }

        .info-icon {
            font-size: 22px;
            flex-shrink: 0;
            margin-top: 1px;
        }

        .info-detail {
            flex: 1;
            font-size: 15px;
            line-height: 1.6;
            color: var(--charcoal);
        }

        .info-detail strong {
            display: block;
            font-weight: 600;
            margin-bottom: 3px;
            font-size: 13px;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            color: var(--leather);
            opacity: 0.9;
        }

        .info-detail a {
            color: var(--rust);
            text-decoration: none;
            font-weight: 600;
            word-break: break-word;
        }

        .cta-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 12px;
            margin-top: 24px;
        }

        .cta-btn {
            height: 52px;
            border: none;
            border-radius: 14px;
            font-size: 15px;
            font-weight: 700;
            font-family: 'Inter', sans-serif;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            transition: all 0.2s ease;
            text-decoration: none;
        }

        .cta-btn:active {
            transform: scale(0.96);
        }

        .cta-btn.primary {
            background: linear-gradient(135deg, var(--rust) 0%, var(--leather) 100%);
            color: var(--white);
            box-shadow: 0 4px 14px rgba(183, 65, 14, 0.3);
        }

        .cta-btn.secondary {
            background: var(--sage);
            color: var(--white);
            box-shadow: 0 4px 14px rgba(156, 175, 136, 0.3);
        }

        .loading-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: var(--cream);
            background-image: var(--grain);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 20px;
            z-index: 5000;
            transition: opacity 0.5s ease;
        }

        .loading-overlay.hide {
            opacity: 0;
            pointer-events: none;
        }

        .loader-spinner {
            width: 48px;
            height: 48px;
            border: 3px solid var(--sand);
            border-top-color: var(--rust);
            border-radius: 50%;
            animation: spin 0.8s linear infinite;
        }

        .loading-text {
            font-size: 15px;
            font-weight: 600;
            color: var(--leather);
            opacity: 0.7;
        }

        .empty-message {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            text-align: center;
            z-index: 500;
            display: none;
            flex-direction: column;
            align-items: center;
            gap: 12px;
            padding: 0 32px;
        }

        .empty-message.show {
            display: flex;
        }

        .empty-icon {
            font-size: 56px;
            opacity: 0.25;
        }

        .empty-text {
            font-size: 16px;
            color: var(--leather);
            font-weight: 600;
        }

        .toast {
            position: fixed;
            bottom: 100px;
            left: 50%;
            transform: translateX(-50%) translateY(200px);
            background: var(--charcoal);
            color: var(--white);
            padding: 14px 24px;
            border-radius: 12px;
            font-size: 14px;
            font-weight: 600;
            z-index: 3000;
            box-shadow: 0 6px 20px var(--shadow-lg);
            transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            max-width: calc(100% - 40px);
            text-align: center;
        }

        .toast.show {
            transform: translateX(-50%) translateY(0);
        }

        @media (min-width: 768px) {
            .sticky-header {
                max-width: 480px;
                left: 20px;
                top: 20px;
                border-radius: 20px;
            }

            .bottom-sheet {
                max-width: 480px;
                left: 20px;
                border-radius: 28px;
                max-height: calc(100vh - 40px);
                bottom: 20px;
            }

            .fab-near-me {
                bottom: 20px;
                right: 20px;
            }

            .toast {
                bottom: 40px;
            }
        }
    </style>
</head>
<body>
    <div class="loading-overlay" id="loader">
        <div class="loader-spinner"></div>
        <div class="loading-text">Loading Pendleton...</div>
    </div>

    <header class="sticky-header">
        <div class="header-content">
            <div class="brand-row">
                <div class="brand">
                    <div class="brand-icon">🤠</div>
                    <div class="brand-text">
                        <h1>Pendleton Local</h1>
                        <p>Discover Downtown</p>
                    </div>
                </div>
            </div>
            <div class="search-container">
                <input type="text" class="search-input" id="searchInput" placeholder="Search businesses..." autocomplete="off">
                <button class="search-clear" id="searchClear" aria-label="Clear search">×</button>
                <span class="search-icon">🔍</span>
            </div>
        </div>
        <div class="filter-scroll" id="categoryFilters">
            <button class="filter-chip active" data-category="all">All</button>
            <button class="filter-chip food" data-category="food"><span>🍽️</span><span>Food</span></button>
            <button class="filter-chip retail" data-category="retail"><span>🛍️</span><span>Retail</span></button>
            <button class="filter-chip art" data-category="art"><span>🎨</span><span>Art</span></button>
            <button class="filter-chip entertainment" data-category="entertainment"><span>🎭</span><span>Events</span></button>
            <button class="filter-chip coffee" data-category="coffee"><span>☕</span><span>Coffee</span></button>
            <button class="filter-chip icecream" data-category="icecream"><span>🍦</span><span>Park</span></button>
        </div>
    </header>

    <div id="map"></div>

    <button class="fab-near-me" id="nearMeBtn" aria-label="Find businesses near me">
        <span>📍</span>
    </button>

    <div class="empty-message" id="emptyMsg">
        <div class="empty-icon">🔍</div>
        <div class="empty-text">No businesses match your filter</div>
    </div>

    <div class="bottom-sheet" id="bizSheet">
        <div class="sheet-header">
            <div class="sheet-handle"></div>
            <button class="sheet-close" id="closeSheet" aria-label="Close">×</button>
        </div>
        <div class="sheet-content" id="sheetContent"></div>
    </div>

    <div class="toast" id="toast"></div>

    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>
    <script>
        // Business data - 10 Pendleton, OR locations across 6 categories
        // Replace youtubeId with the 11-character ID from your YouTube URL
        // Example: https://www.youtube.com/watch?v=dQw4w9WgXcQ -> youtubeId: "dQw4w9WgXcQ"
        const BUSINESSES = [
            {
                id: 1,
                name: "LL Bevington",
                category: "retail",
                lat: 45.67079,
                lng: -118.78653,
                address: "365 S Main St, Pendleton, OR 97801",
                phone: "(541) 276-6151",
                website: "https://www.llbevington.com",
                hours: "Mon-Sat: 9AM-5PM, Sun: Closed",
                description: "Custom leather design studio and boutique in downtown Pendleton one of a kind vests, purses, bags, totes, and home goods like pillows and table runners.",
                youtubeId: "izQl_3kOv6o"
            },
            {
                id: 2,
                name: "Morningstar Blends",
                category: "retail",
                lat: 45.67097,
                lng: -118.78660,
                address: "347 S Main St, Pendleton, OR 97801",
                phone: "(541) 303-4644",
                website: "http://www.morningstarblends.com/",
                hours: "Sun: 11AM-4PM, Mon-Thu: 10AM-6PM, Fri: 10AM-7PM, Sat: 10AM-6PM",
                description: "Wildcrafted, organic, & fair trade herbs made into potent herbal teas. LLC offering herbal blends, tea steepers, honey pot sets, storage containers, and products supporting Pollinator Partnership. Known for Tea Tuesday 10% off blends and flower crown craft events.",
                youtubeId: ""
            },
            {
                id: 3,
                name: "Rainbow Cafe",
                category: "food",
                lat: 45.67244,
                lng: -118.78765,
                address: "209 S Main St, Pendleton, OR 97801",
                phone: "(541) 276-4120",
                website: "https://thependletonrainbow.com/",
                hours: "Sun: 6AM-12AM, Mon-Sat: 6AM-2AM",
                description: "The oldest Tavern in Oregon operating since 1883. A true cowboy bar in the heart of rodeo town serving hearty American fare, breakfast highlights like Chuckwagon Benedict.",
                youtubeId: ""
            },
            {
                id:4,
                name: "Great Pacific Wine & Coffee",
                category: "food",
                lat: 45.67047,
                lng: -118.78633,
                address: "403 S Main St, Pendleton, OR 97801",
                phone: "+1 (541) 276-1350",
                website: "http://www.greatpacific.biz",
                hours: "Mon-Sat: 11AM-9PM",
                description: "Downtown gem with pizza, sandwiches, salads, wine, and cocktails. Order at counter, indoor/outdoor pet-friendly seating. Known for craft cocktails and welcoming vibe.",
                youtubeId: ""

            },
            {
                id: 5,
                name: "VYBE & Eden's Kitchen",
                category: "food",
                lat: 45.67203,
                lng: -118.78756,
                address: "249 South Main Street, Pendleton, OR 97801",
                phone: "",
                website: "",
                hours: "6PM-2AM, last call 1AM",
                description: "New urban-core bar scene with inclusive mocktails and Eden's Kitchen serving fresh homemade sammies. Grand opening April 30th with Chamber ribbon-cutting.",
                youtubeId: ""

            },
            {
                id: 6,
                name: "Crow Shadow Institute",
                category: "retail",
                lat: 45.6750,
                lng: -118.7900,
                address: "202 S Main St, Pendleton, OR 97801",
                phone: "(541) 276-3954",
                website: "https://crowshadow.org",
                hours: "By Appointment",
                description: "Native American art printmaking studio and gallery showcasing contemporary Indigenous artists from the Umatilla tribes.",
                youtubeId: "fJ9rUzIMcZQ"
            },
            {
                id: 7,
                name: "Pendleton Round-Up",
                category: "entertainment",
                lat: 45.6678,
                lng: -118.8001,
                address: "1205 SW Court Ave, Pendleton, OR 97801",
                phone: "(541) 276-2553",
                website: "https://pendletonroundup.com",
                hours: "Event Hours Vary - September Rodeo",
                description: "World-famous rodeo arena hosting the legendary Pendleton Round-Up each September since 1910. Let 'er buck!",
                youtubeId: "L_jWHffIx5E"
            },
            {
                id: 8,
                name: "Vert Auditorium",
                category: "entertainment",
                lat: 45.6690,
                lng: -118.7885,
                address: "480 SW Dorion Ave, Pendleton, OR 97801",
                phone: "(541) 966-0326",
                website: "https://pendletonparksandrec.com",
                hours: "Events Only - Check Schedule",
                description: "Historic community theater hosting concerts, performances, and events year-round in downtown Pendleton.",
                youtubeId: "RgKAFK5djSk"
            },
            {
                id: 9,
                name: "Great Pacific Wine & Coffee",
                category: "food",
                lat: 45.6722,
                lng: -118.7889,
                address: "403 S Main St, Pendleton, OR 97801",
                phone: "(541) 276-7111",
                website: "https://greatpacific.com",
                hours: "Mon-Fri: 7AM-6PM, Sat-Sun: 8AM-5PM",
                description: "Local roastery and wine bar featuring craft coffee, regional wines, fresh pastries, and cozy community space.",
                youtubeId: "astISOttCQ0"
            },
            {
                id: 10,
                name: "Til Taylor Park",
                category: "icecream",
                lat: 45.6745,
                lng: -118.7865,
                address: "727 SE 1st St, Pendleton, OR 97801",
                phone: "(541) 276-8585",
                website: "https://wildhorseresort.com",
                hours: "Daily: 11AM-9PM",
                description: "Splash pad, swings, play structure.",
                youtubeId: "H-wTzw70B-E"
            }
        ];

        const CATEGORY_CONFIG = {
            food: { emoji: '🍽️', label: 'Food' },
            retail: { emoji: '🛍️', label: 'Retail' },
            art: { emoji: '🎨', label: 'Art' },
            entertainment: { emoji: '🎭', label: 'Entertainment' },
            coffee: { emoji: '☕', label: 'Coffee' },
            icecream: { emoji: '🍦', label: 'Ice Cream' }
        };

        let map;
        let markers = [];
        let activeFilter = 'all';
        let userLocation = null;
        let userLocationMarker = null;
        let activeMarker = null;

        function initMap() {
            try {
                if (typeof L === 'undefined') {
                    showToast('Map library failed to load. Please refresh.');
                    document.getElementById('loader').classList.add('hide');
                    return;
                }

                map = L.map('map', {
                    center: [45.6721, -118.7886],
                    zoom: 14,
                    zoomControl: true,
                    attributionControl: true,
                    tap: false
                });

                L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
                    maxZoom: 19,
                    attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a>',
                    errorTileUrl: 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mNk+M9QDwADhgGAWjR9awAAAABJRU5ErkJggg=='
                }).addTo(map);

                renderBusinessMarkers(BUSINESSES);
                initEventListeners();

                setTimeout(() => {
                    document.getElementById('loader').classList.add('hide');
                }, 600);

            } catch (err) {
                console.error('Map init failed:', err);
                showToast('Failed to load map. Please refresh the page.');
                document.getElementById('loader').classList.add('hide');
            }
        }

        function createMarkerIcon(business, isActive = false) {
            const config = CATEGORY_CONFIG[business.category];
            const className = `custom-marker ${business.category} ${isActive ? 'active' : ''}`;
            return L.divIcon({
                className: '',
                html: `<div class="${className}">${config.emoji}</div>`,
                iconSize: [44, 44],
                iconAnchor: [22, 22]
            });
        }

        function renderBusinessMarkers(businessArray) {
            markers.forEach(m => map.removeLayer(m));
            markers = [];

            businessArray.forEach((biz) => {
                const marker = L.marker([biz.lat, biz.lng], {
                    icon: createMarkerIcon(biz),
                    riseOnHover: false
                }).addTo(map);

                marker.businessData = biz;
                marker.businessId = biz.id;

                marker.on('click', () => {
                    openBusinessSheet(biz);
                    highlightMarker(biz.id);
                });

                markers.push(marker);
            });

            document.getElementById('emptyMsg').classList.toggle('show', businessArray.length === 0);
        }

        function highlightMarker(bizId) {
            markers.forEach(m => {
                const isActive = m.businessId === bizId;
                m.setIcon(createMarkerIcon(m.businessData, isActive));
                if (isActive) {
                    activeMarker = m;
                    m.setZIndexOffset(1000);
                } else {
                    m.setZIndexOffset(0);
                }
            });
        }

        function filterBusinesses(category, searchQuery = '') {
            let filtered = BUSINESSES;

            if (category !== 'all') {
                filtered = filtered.filter(b => b.category === category);
            }

            if (searchQuery.trim()) {
                const q = searchQuery.toLowerCase();
                filtered = filtered.filter(b => 
                    b.name.toLowerCase().includes(q) ||
                    b.description.toLowerCase().includes(q) ||
                    b.address.toLowerCase().includes(q)
                );
            }

            renderBusinessMarkers(filtered);
            return filtered;
        }

        function openBusinessSheet(biz) {
            const sheet = document.getElementById('bizSheet');
            const content = document.getElementById('sheetContent');
            const config = CATEGORY_CONFIG[biz.category];
            const phoneClean = biz.phone.replace(/[^0-9]/g, '');
            const youtubeSrc = `https://www.youtube.com/embed/${biz.youtubeId}?autoplay=1&mute=1&playsinline=1&controls=1&rel=0&modestbranding=1`;
            
            content.innerHTML = `
                <div class="video-wrapper">
                    <iframe 
                        id="ytPlayer"
                        src="${youtubeSrc}"
                        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
                        allowfullscreen
                        title="${biz.name} Video">
                    </iframe>
                </div>
                <div class="video-note">🔇 Video starts muted. Tap video for sound.</div>
                <div class="biz-header">
                    <div class="category-badge ${biz.category}">
                        <span>${config.emoji}</span>
                        <span>${config.label}</span>
                    </div>
                    <h2 class="biz-name">${biz.name}</h2>
                    <p class="biz-description">${biz.description}</p>
                </div>
                <div class="info-grid">
                    <div class="info-row">
                        <div class="info-icon">📍</div>
                        <div class="info-detail">
                            <strong>Address</strong>
                            <a href="https://www.google.com/maps/dir/?api=1&destination=${encodeURIComponent(biz.address)}" target="_blank" rel="noopener">${biz.address}</a>
                        </div>
                    </div>
                    <div class="info-row">
                        <div class="info-icon">📞</div>
                        <div class="info-detail">
                            <strong>Phone</strong>
                            <a href="tel:${phoneClean}">${biz.phone}</a>
                        </div>
                    </div>
                    <div class="info-row">
                        <div class="info-icon">🌐</div>
                        <div class="info-detail">
                            <strong>Website</strong>
                            <a href="${biz.website}" target="_blank" rel="noopener">Visit Website</a>
                        </div>
                    </div>
                    <div class="info-row">
                        <div class="info-icon">🕐</div>
                        <div class="info-detail">
                            <strong>Hours</strong>
                            ${biz.hours}
                        </div>
                    </div>
                </div>
                <div class="cta-grid">
                    <a class="cta-btn primary" href="https://www.google.com/maps/dir/?api=1&destination=${encodeURIComponent(biz.address)}" target="_blank" rel="noopener">
                        <span>🧭</span>
                        <span>Directions</span>
                    </a>
                    <a class="cta-btn secondary" href="tel:${phoneClean}">
                        <span>📞</span>
                        <span>Call Now</span>
                    </a>
                </div>
            `;

            sheet.classList.add('show');
        }

        function closeBusinessSheet() {
            const sheet = document.getElementById('bizSheet');
            const iframe = document.getElementById('ytPlayer');
            if (iframe) {
                iframe.src = ''; // Stop YouTube playback
            }
            sheet.classList.remove('show');
            
            markers.forEach(m => {
                m.setIcon(createMarkerIcon(m.businessData, false));
                m.setZIndexOffset(0);
            });
            activeMarker = null;
        }

        function findNearMe() {
            if (!navigator.geolocation) {
                showToast('Geolocation not supported by your browser');
                return;
            }

            const btn = document.getElementById('nearMeBtn');
            btn.classList.add('loading');
            btn.disabled = true;

            navigator.geolocation.getCurrentPosition(
                (position) => {
                    userLocation = [position.coords.latitude, position.coords.longitude];

                    if (userLocationMarker) {
                        map.removeLayer(userLocationMarker);
                    }

                    userLocationMarker = L.marker(userLocation, {
                        icon: L.divIcon({
                            className: '',
                            html: '<div class="user-marker"></div>',
                            iconSize: [20, 20],
                            iconAnchor: [10, 10]
                        }),
                        zIndexOffset: 2000
                    }).addTo(map);

                    map.setView(userLocation, 15, { animate: true });

                    const sorted = [...BUSINESSES].sort((a, b) => {
                        const distA = map.distance(userLocation, [a.lat, a.lng]);
                        const distB = map.distance(userLocation, [b.lat, b.lng]);
                        return distA - distB;
                    });

                    renderBusinessMarkers(sorted);
                    btn.classList.remove('loading');
                    btn.disabled = false;
                    showToast('Showing businesses near you');
                },
                (error) => {
                    btn.classList.remove('loading');
                    btn.disabled = false;
                    let message = 'Unable to get your location. ';
                    if (error.code === 1) message += 'Please enable location permissions.';
                    else if (error.code === 2) message += 'Location unavailable.';
                    else message += 'Request timed out.';
                    showToast(message);
                },
                {
                    enableHighAccuracy: true,
                    timeout: 10000,
                    maximumAge: 0
                }
            );
        }

        function showToast(message) {
            const toast = document.getElementById('toast');
            toast.textContent = message;
            toast.classList.add('show');
            setTimeout(() => {
                toast.classList.remove('show');
            }, 3000);
        }

        function initEventListeners() {
            document.getElementById('closeSheet').addEventListener('click', closeBusinessSheet);
            document.getElementById('nearMeBtn').addEventListener('click', findNearMe);

            document.querySelectorAll('.filter-chip').forEach(chip => {
                chip.addEventListener('click', (e) => {
                    document.querySelectorAll('.filter-chip').forEach(c => c.classList.remove('active'));
                    e.currentTarget.classList.add('active');
                    activeFilter = e.currentTarget.dataset.category;
                    const query = document.getElementById('searchInput').value;
                    filterBusinesses(activeFilter, query);
                });
            });

            const searchInput = document.getElementById('searchInput');
            const searchClear = document.getElementById('searchClear');
            
            searchInput.addEventListener('input', (e) => {
                const query = e.target.value;
                searchClear.classList.toggle('show', query.length > 0);
                filterBusinesses(activeFilter, query);
            });

            searchClear.addEventListener('click', () => {
                searchInput.value = '';
                searchClear.classList.remove('show');
                filterBusinesses(activeFilter, '');
                searchInput.focus();
            });

            map.on('click', closeBusinessSheet);

            let touchStartY = 0;
            let touchStartScroll = 0;
            const sheet = document.getElementById('bizSheet');
            const sheetContent = document.getElementById('sheetContent');
            
            sheet.addEventListener('touchstart', (e) => {
                touchStartY = e.touches[0].clientY;
                touchStartScroll = sheetContent.scrollTop;
            }, { passive: true });

            sheet.addEventListener('touchmove', (e) => {
                const touchY = e.touches[0].clientY;
                const diff = touchY - touchStartY;
                if (diff > 80 && touchStartScroll === 0) {
                    closeBusinessSheet();
                }
            }, { passive: true });
        }

        if (document.readyState === 'loading') {
            document.addEventListener('DOMContentLoaded', initMap);
        } else {
            initMap();
        }
    </script>
<script>(function(){document.addEventListener("click",function(e){var a=e.target.closest("[data-product-id]");if(!a)return;e.preventDefault();var pid=a.getAttribute("data-product-id");if(pid)parent.postMessage({type:"ecto-artifact-link-click",productId:pid},"*")})})();</script>
</body>
</html>
