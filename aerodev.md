<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AeroDev Solutions - Digital Business Card</title>
<script src="https://cdn.jsdelivr.net/npm/qrcode@1.5.3/build/qrcode.min.js"></script>
<style>
    :root {
        --primary: #0ea5e9;
        --primary-dark: #0284c7;
        --secondary: #38bdf8;
        --accent: #0c4a6e;
        --text: #1e293b;
        --text-light: #64748b;
        --bg: #f0f9ff;
        --card: rgba(255, 255, 255, 0.8);
        --border: rgba(14, 165, 233, 0.2);
        --shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.15);
    }

    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
    }

    body {
        font-family: 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
        background: linear-gradient(135deg, #e0f2fe 0%, #bae6fd 25%, #7dd3fc 50%, #38bdf8 100%);
        background-attachment: fixed;
        min-height: 100vh;
        color: var(--text);
        padding: 20px 12px;
        position: relative;
        overflow-x: hidden;
    }

    /* Animated clouds background */
    body::before {
        content: '';
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-image: 
            radial-gradient(circle at 20% 30%, rgba(255,255,255,0.4) 0%, transparent 50%),
            radial-gradient(circle at 80% 70%, rgba(255,255,255,0.3) 0%, transparent 50%),
            radial-gradient(circle at 40% 80%, rgba(255,255,255,0.3) 0%, transparent 40%);
        animation: drift 60s linear infinite;
        pointer-events: none;
        z-index: 0;
    }

    @keyframes drift {
        from { transform: translateX(0); }
        to { transform: translateX(100px); }
    }

    .container {
        max-width: 480px;
        margin: 0 auto;
        position: relative;
        z-index: 1;
    }

    .card {
        background: var(--card);
        backdrop-filter: blur(20px);
        -webkit-backdrop-filter: blur(20px);
        border-radius: 24px;
        border: 1px solid var(--border);
        box-shadow: var(--shadow);
        padding: 32px 24px;
        margin-bottom: 20px;
        transition: transform 0.3s ease, box-shadow 0.3s ease;
    }

    .card:hover {
        transform: translateY(-4px);
        box-shadow: 0 12px 40px 0 rgba(31, 38, 135, 0.2);
    }

    /* Profile Section */
    .profile {
        text-align: center;
        position: relative;
    }

    .avatar-wrapper {
        position: relative;
        width: 120px;
        height: 120px;
        margin: 0 auto 20px;
    }

    .avatar {
        width: 120px;
        height: 120px;
        border-radius: 50%;
        background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
        display: flex;
        align-items: center;
        justify-content: center;
        border: 4px solid rgba(255, 255, 255, 0.9);
        box-shadow: 0 8px 24px rgba(14, 165, 233, 0.3);
        overflow: hidden;
    }

    .avatar svg {
        width: 60px;
        height: 60px;
        fill: white;
    }

    .status-badge {
        position: absolute;
        bottom: 8px;
        right: 8px;
        width: 24px;
        height: 24px;
        background: #22c55e;
        border: 3px solid white;
        border-radius: 50%;
        animation: pulse 2s infinite;
    }

    @keyframes pulse {
        0%, 100% { box-shadow: 0 0 0 0 rgba(34, 197, 94, 0.7); }
        50% { box-shadow: 0 0 0 8px rgba(34, 197, 94, 0); }
    }

    .business-name {
        font-size: 28px;
        font-weight: 700;
        margin-bottom: 8px;
        color: var(--accent);
        letter-spacing: -0.5px;
    }

    .tagline {
        font-size: 14px;
        color: var(--text-light);
        font-weight: 500;
        text-transform: uppercase;
        letter-spacing: 1px;
        margin-bottom: 20px;
    }

    /* Contrail Divider */
    .contrail {
        display: flex;
        align-items: center;
        justify-content: center;
        margin: 24px 0;
        opacity: 0.6;
    }

    .contrail::before,
    .contrail::after {
        content: '';
        flex: 1;
        height: 2px;
        background: linear-gradient(90deg, transparent, var(--primary), transparent);
    }

    .contrail svg {
        width: 24px;
        height: 24px;
        margin: 0 12px;
        fill: var(--primary);
        transform: rotate(45deg);
    }

    /* Bio Section */
    .bio {
        font-size: 15px;
        line-height: 1.7;
        color: var(--text);
        text-align: center;
        margin-bottom: 8px;
    }

    /* Contact Buttons */
    .section-title {
        font-size: 13px;
        font-weight: 700;
        text-transform: uppercase;
        letter-spacing: 1.5px;
        color: var(--primary-dark);
        margin-bottom: 16px;
        display: flex;
        align-items: center;
        gap: 8px;
    }

    .contact-grid {
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        gap: 12px;
        margin-bottom: 8px;
    }

    .contact-grid .btn-full {
        grid-column: 1 / -1;
    }

    .btn {
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 10px;
        padding: 14px 20px;
        border-radius: 14px;
        text-decoration: none;
        font-weight: 600;
        font-size: 15px;
        transition: all 0.3s ease;
        border: 2px solid transparent;
        cursor: pointer;
        position: relative;
        overflow: hidden;
    }

    .btn::before {
        content: '';
        position: absolute;
        top: 50%;
        left: 50%;
        width: 0;
        height: 0;
        border-radius: 50%;
        background: rgba(255, 255, 255, 0.3);
        transform: translate(-50%, -50%);
        transition: width 0.6s, height 0.6s;
    }

    .btn:active::before {
        width: 300px;
        height: 300px;
    }

    .btn svg {
        width: 20px;
        height: 20px;
        flex-shrink: 0;
    }

    .btn-primary {
        background: linear-gradient(135deg, var(--primary) 0%, var(--primary-dark) 100%);
        color: white;
        box-shadow: 0 4px 14px rgba(14, 165, 233, 0.4);
    }

    .btn-primary:hover {
        transform: translateY(-2px);
        box-shadow: 0 6px 20px rgba(14, 165, 233, 0.5);
    }

    .btn-secondary {
        background: white;
        color: var(--primary-dark);
        border-color: var(--border);
    }

    .btn-secondary:hover {
        background: var(--bg);
        border-color: var(--primary);
        transform: translateY(-2px);
    }

    /* Social Links */
    .social-links {
        display: flex;
        justify-content: center;
        gap: 12px;
        flex-wrap: wrap;
    }

    .social-btn {
        width: 50px;
        height: 50px;
        border-radius: 50%;
        background: white;
        display: flex;
        align-items: center;
        justify-content: center;
        text-decoration: none;
        transition: all 0.3s ease;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
        border: 2px solid transparent;
    }

    .social-btn:hover {
        transform: translateY(-4px) scale(1.05);
        box-shadow: 0 6px 16px rgba(0, 0, 0, 0.12);
        border-color: var(--primary);
    }

    .social-btn svg {
        width: 22px;
        height: 22px;
        fill: var(--primary-dark);
    }

    /* Services */
    .services-list {
        display: flex;
        flex-direction: column;
        gap: 12px;
    }

    .service-item {
        display: flex;
        align-items: center;
        gap: 14px;
        padding: 14px;
        background: white;
        border-radius: 12px;
        border: 1px solid var(--border);
        transition: all 0.3s ease;
    }

    .service-item:hover {
        transform: translateX(4px);
        border-color: var(--primary);
        box-shadow: 0 4px 12px rgba(14, 165, 233, 0.15);
    }

    .service-icon {
        width: 44px;
        height: 44px;
        border-radius: 10px;
        background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
        display: flex;
        align-items: center;
        justify-content: center;
        flex-shrink: 0;
    }

    .service-icon svg {
        width: 22px;
        height: 22px;
        fill: white;
    }

    .service-content h3 {
        font-size: 15px;
        font-weight: 600;
        color: var(--text);
        margin-bottom: 2px;
    }

    .service-content p {
        font-size: 13px;
        color: var(--text-light);
        line-height: 1.4;
    }

    /* QR Code Section */
    .qr-section {
        text-align: center;
    }

    .qr-wrapper {
        background: white;
        padding: 20px;
        border-radius: 16px;
        display: inline-block;
        box-shadow: 0 4px 16px rgba(0, 0, 0, 0.08);
        margin: 16px 0;
    }

    #qrcode {
        display: flex;
        justify-content: center;
    }

    .qr-text {
        font-size: 13px;
        color: var(--text-light);
        margin-top: 12px;
    }

    /* Footer */
    .footer {
        text-align: center;
        padding: 24px 0 12px;
        font-size: 14px;
        color: var(--accent);
        font-weight: 500;
    }

    .footer-icon {
        display: inline-block;
        animation: bounce 2s infinite;
    }

    @keyframes bounce {
        0%, 100% { transform: translateY(0); }
        50% { transform: translateY(-5px); }
    }

    /* Responsive */
    @media (max-width: 390px) {
        .card {
            padding: 24px 18px;
        }
        
        .business-name {
            font-size: 24px;
        }
        
        .contact-grid {
            grid-template-columns: 1fr;
        }
        
        .btn {
            padding: 12px 16px;
            font-size: 14px;
        }
    }

    /* Toast Notification */
    .toast {
        position: fixed;
        bottom: 24px;
        left: 50%;
        transform: translateX(-50%) translateY(100px);
        background: var(--accent);
        color: white;
        padding: 12px 24px;
        border-radius: 12px;
        box-shadow: 0 8px 24px rgba(0, 0, 0, 0.2);
        font-weight: 600;
        font-size: 14px;
        z-index: 1000;
        opacity: 0;
        transition: all 0.3s ease;
    }

    .toast.show {
        transform: translateX(-50%) translateY(0);
        opacity: 1;
    }
</style>
</head>
<body>
    <div class="container">
        <!-- Profile Card -->
        <div class="card profile">
            <div class="avatar-wrapper">
                <div class="avatar">
                    <!-- EDIT: Replace with your logo image: <img src="YOUR_LOGO_URL" alt="Logo" style="width:100%;height:100%;object-fit:cover;"> -->
                    <svg viewBox="0 0 24 24">
                        <path d="M21,16V14L13,9V3.5A1.5,1.5 0 0,0 11.5,2A1.5,1.5 0 0,0 10,3.5V9L2,14V16L10,13.5V19L8,20.5V22L11.5,21L15,22V20.5L13,19V13.5L21,16Z"/>
                    </svg>
                </div>
                <div class="status-badge" title="Available Now"></div>
            </div>
            
            <!-- EDIT: YOUR_NAME -->
            <h1 class="business-name">AeroDev Solutions</h1>
            
            <!-- EDIT: YOUR_TAGLINE -->
            <p class="tagline">Tech Consultations | Video | Drone Automation</p>

            <div class="contrail">
                <svg viewBox="0 0 24 24">
                    <path d="M21,16V14L13,9V3.5A1.5,1.5 0 0,0 11.5,2A1.5,1.5 0 0,0 10,3.5V9L2,14V16L10,13.5V19L8,20.5V22L11.5,21L15,22V20.5L13,19V13.5L21,16Z"/>
                </svg>
            </div>

            <!-- EDIT: YOUR_BIO -->
            <p class="bio">
                Elevating businesses through cutting-edge web development, aerial cinematography, and automation solutions. We blend aviation precision with modern tech to help you soar above the competition.
            </p>
        </div>

        <!-- Contact Buttons Card -->
        <div class="card">
            <div class="section-title">
                <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor">
                    <path d="M20,8L12,13L4,8V6L12,11L20,6M20,4H4C2.89,4 2,4.89 2,6V18A2,2 0 0,0 4,20H20A2,2 0 0,0 22,18V6C22,4.89 21.1,4 20,4Z"/>
                </svg>
                Connect Instantly
            </div>
            <div class="contact-grid">
                <!-- EDIT: YOUR_PHONE -->
                <a href="tel:+15551234567" class="btn btn-primary">
                    <svg viewBox="0 0 24 24" fill="currentColor">
                        <path d="M6.62,10.79C8.06,13.62 10.38,15.94 13.21,17.38L15.41,15.18C15.69,14.9 16.08,14.82 16.43,14.93C17.55,15.3 18.75,15.5 20,15.5A1,1 0 0,1 21,16.5V20A1,1 0 0,1 20,21A17,17 0 0,1 3,4A1,1 0 0,1 4,3H7.5A1,1 0 0,1 8.5,4C8.5,5.25 8.7,6.45 9.07,7.57C9.18,7.92 9.1,8.31 8.82,8.59L6.62,10.79Z"/>
                    </svg>
                    Call
                </a>
                
                <!-- EDIT: YOUR_EMAIL -->
                <a href="mailto:hello@aerodev.com" class="btn btn-secondary">
                    <svg viewBox="0 0 24 24" fill="currentColor">
                        <path d="M20,8L12,13L4,8V6L12,11L20,6M20,4H4C2.89,4 2,4.89 2,6V18A2,2 0 0,0 4,20H20A2,2 0 0,0 22,18V6C22,4.89 21.1,4 20,4Z"/>
                    </svg>
                    Email
                </a>
                
                <!-- EDIT: YOUR_PHONE -->
                <a href="sms:+15551234567" class="btn btn-secondary">
                    <svg viewBox="0 0 24 24" fill="currentColor">
                        <path d="M20,2H4A2,2 0 0,0 2,4V22L6,18H20A2,2 0 0,0 22,16V4C22,2.89 21.1,2 20,2M20,16H6L4,18V4H20"/>
                    </svg>
                    SMS
                </a>
                
                <!-- EDIT: YOUR_WHATSAPP_NUMBER (without + or spaces) -->
                <a href="https://wa.me/15551234567" target="_blank" class="btn btn-secondary">
                    <svg viewBox="0 0 24 24" fill="currentColor">
                        <path d="M12.04 2C6.58 2 2.13 6.45 2.13 11.91C2.13 13.66 2.59 15.36 3.45 16.86L2.05 22L7.3 20.62C8.75 21.41 10.38 21.83 12.04 21.83C17.5 21.83 21.95 17.38 21.95 11.92C21.95 9.27 20.92 6.78 19.05 4.91C17.18 3.03 14.69 2 12.04 2M12.05 3.67C14.25 3.67 16.31 4.53 17.87 6.09C19.42 7.65 20.28 9.72 20.28 11.92C20.28 16.46 16.58 20.15 12.04 20.15C10.56 20.15 9.11 19.76 7.85 19.05L7.56 18.88L4.43 19.7L5.26 16.67L5.06 16.36C4.29 15.04 3.86 13.5 3.86 11.91C3.86 7.37 7.55 3.67 12.05 3.67M8.53 7.33C8.37 7.33 8.1 7.39 7.87 7.64C7.65 7.89 7 8.5 7 9.71C7 10.93 7.89 12.1 8 12.27C8.14 12.44 9.76 14.94 12.25 16C12.84 16.27 13.3 16.42 13.66 16.53C14.25 16.72 14.79 16.69 15.22 16.63C15.7 16.56 16.68 16.03 16.88 15.45C17.08 14.87 17.08 14.38 17.04 14.28C16.97 14.18 16.81 14.12 16.56 14C16.31 13.86 15.09 13.26 14.87 13.18C14.64 13.1 14.5 13.06 14.31 13.3C14.15 13.55 13.67 14.11 13.53 14.27C13.39 14.44 13.25 14.46 13 14.34C12.75 14.23 11.9 13.95 10.89 13.05C10.06 12.32 9.5 11.42 9.35 11.16C9.22 10.91 9.34 10.76 9.46 10.64C9.57 10.53 9.7 10.36 9.82 10.23C9.94 10.1 9.98 10 10.07 9.84C10.15 9.68 10.11 9.55 10.05 9.42C9.99 9.3 9.49 8.08 9.24 7.58C9.02 7.11 8.77 7.2 8.59 7.19C8.44 7.19 8.27 7.19 8.1 7.19C8.53 7.33 8.53 7.33 8.53 7.33Z"/>
                    </svg>
                    WhatsApp
                </a>
                
                <button onclick="downloadVCard()" class="btn btn-primary btn-full">
                    <svg viewBox="0 0 24 24" fill="currentColor">
                        <path d="M15,9H5V5H15M12,19A3,3 0 0,1 9,16A3,3 0 0,1 12,13A3,3 0 0,1 15,16A3,3 0 0,1 12,19M17,3H5C3.89,3 3,3.9 3,5V19A2,2 0 0,0 5,21H19A2,2 0 0,0 21,19V7L17,3Z"/>
                    </svg>
                    Save Contact
                </button>
            </div>
        </div>

        <!-- Social Links Card -->
        <div class="card">
            <div class="section-title">
                <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor">
                    <path d="M13.5 2C13.5 2.5 13.37 2.98 13.13 3.41L13.38 6.5H10.61L7.75 3.41C7.5 2.98 7.25 2.5 7.25 2H10.5C10.5 2 10.5 2.5 10.25 2.94L10 6H11.12L11.38 9H9.5L6.64 5.94C6.39 6.38 6.14 6.86 5.89 7.33L9.63 11.07L13.38 7.33C13.13 6.86 12.88 6.38 12.63 5.94L9.77 9H11.64L11.89 12H10.77L10.52 15H12.64L12.89 18H10.27L10 21H11.12L11.37 18H13.5L13.75 21H14.88L15.13 18H17.26L17.51 15H15.38L15.63 12H17.76L18.01 9H15.88L16.13 6H18.25L18.5 3.41C18.26 2.98 18.13 2.5 18.13 2H21.38C21.38 2.5 21.25 2.98 21.01 3.41L21.26 6.5H18.5L15.64 3.41C15.39 2.98 15.25 2.5 15.25 2H13.5Z"/>
                </svg>
                Follow Us
            </div>
            <div class="social-links">
                <!-- EDIT: YOUR_LINKEDIN_URL -->
                <a href="https://linkedin.com/company/yourcompany" target="_blank" class="social-btn" title="LinkedIn">
                    <svg viewBox="0 0 24 24"><path d="M19 3A2 2 0 0 1 21 5V19A2 2 0 0 1 19 21H5A2 2 0 0 1 3 19V5A2 2 0 0 1 5 3H19M18.5 18.5V13.2A3.26 3.26 0 0 0 15.24 9.94C14.39 9.94 13.4 10.46 12.92 11.24V10.13H10.13V18.5H12.92V13.57C12.92 12.8 13.54 12.17 14.31 12.17A1.4 1.4 0 0 1 15.71 13.57V18.5H18.5M6.88 8.56A1.68 1.68 0 0 0 8.56 6.88C8.56 5.95 7.81 5.19 6.88 5.19A1.69 1.69 0 0 0 5.19 6.88C5.19 7.81 5.95 8.56 6.88 8.56M8.27 18.5V10.13H5.5V18.5H8.27Z"/></svg>
                </a>
                <!-- EDIT: YOUR_YOUTUBE_URL -->
                <a href="https://youtube.com/@yourchannel" target="_blank" class="social-btn" title="YouTube">
                    <svg viewBox="0 0 24 24"><path d="M10,15L15.19,12L10,9V15M21.56,7.17C21.69,7.64 21.78,8.27 21.84,9.07C21.91,9.87 21.94,10.56 21.94,11.16L22,12C22,14.19 21.84,15.8 21.56,16.83C21.31,17.73 20.73,18.31 19.83,18.56C19.36,18.69 18.5,18.78 17.18,18.84C15.88,18.91 14.69,18.94 13.59,18.94L12,19C7.81,19 5.2,18.84 4.17,18.56C3.27,18.31 2.69,17.73 2.44,16.83C2.31,16.36 2.22,15.73 2.16,14.93C2.09,14.13 2.06,13.44 2.06,12.84L2,12C2,9.81 2.16,8.2 2.44,7.17C2.69,6.27 3.27,5.69 4.17,5.44C4.64,5.31 5.5,5.22 6.82,5.16C8.12,5.09 9.31,5.06 10.41,5.06L12,5C16.19,5 18.8,5.16 19.83,5.44C20.73,5.69 21.31,6.27 21.56,7.17Z"/></svg>
                </a>
                <!-- EDIT: YOUR_INSTAGRAM_URL -->
                <a href="https://instagram.com/yourhandle" target="_blank" class="social-btn" title="Instagram">
                    <svg viewBox="0 0 24 24"><path d="M7.8,2H16.2C19.4,2 22,4.6 22,7.8V16.2A5.8,5.8 0 0,1 16.2,22H7.8C4.6,22 2,19.4 2,16.2V7.8A5.8,5.8 0 0,1 7.8,2M7.6,4A3.6,3.6 0 0,0 4,7.6V16.4C4,18.39 5.61,20 7.6,20H16.4A3.6,3.6 0 0,0 20,16.4V7.6C20,5.61 18.39,4 16.4,4H7.6M17.25,5.5A1.25,1.25 0 0,1 18.5,6.75A1.25,1.25 0 0,1 17.25,8A1.25,1.25 0 0,1 16,6.75A1.25,1.25 0 0,1 17.25,5.5M12,7A5,5 0 0,1 17,12A5,5 0 0,1 12,17A5,5 0 0,1 7,12A5,5 0 0,1 12,7M12,9A3,3 0 0,0 9,12A3,3 0 0,0 12,15A3,3 0 0,0 15,12A3,3 0 0,0 12,9Z"/></svg>
                </a>
                <!-- EDIT: YOUR_WEBSITE_URL -->
                <a href="https://yourwebsite.com" target="_blank" class="social-btn" title="Website">
                    <svg viewBox="0 0 24 24"><path d="M16.36,14C16.44,13.34 16.5,12.68 16.5,12C16.5,11.32 16.44,10.66 16.36,10H19.74C19.9,10.64 20,11.31 20,12C20,12.69 19.9,13.36 19.74,14M14.59,19.56C15.19,18.45 15.65,17.25 15.97,16H18.92C17.96,17.65 16.43,18.93 14.59,19.56M14.34,14H9.66C9.56,13.34 9.5,12.68 9.5,12C9.5,11.32 9.56,10.65 9.66,10H14.34C14.43,10.65 14.5,11.32 14.5,12C14.5,12.68 14.43,13.34 14.34,14M12,19.96C11.17,18.76 10.5,17.43 10.09,16H13.91C13.5,17.43 12.83,18.76 12,19.96M8,8H5.08C6.03,6.34 7.57,5.06 9.4,4.44C8.8,5.55 8.35,6.75 8,8M5.08,16H8C8.35,17.25 8.8,18.45 9.4,19.56C7.57,18.93 6.03,17.65 5.08,16M4.26,14C4.1,13.36 4,12.69 4,12C4,11.31 4.1,10.64 4.26,10H7.64C7.56,10.66 7.5,11.32 7.5,12C7.5,12.68 7.56,13.34 7.64,14M12,4.03C12.83,5.23 13.5,6.57 13.91,8H10.09C10.5,6.57 11.17,5.23 12,4.03M18.92,8H15.97C15.65,6.75 15.19,5.55 14.59,4.44C16.43,5.07 17.96,6.34 18.92,8M12,2C6.47,2 2,6.5 2,12A10,10 0 0,0 12,22A10,10 0 0,0 22,12A10,10 0 0,0 12,2Z"/></svg>
                </a>
            </div>
        </div>

        <!-- Services Card -->
        <div class="card">
            <div class="section-title">
                <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor">
                    <path d="M12,15.39L8.24,17.66L9.23,13.38L5.91,10.5L10.29,10.13L12,6.09L13.71,10.13L18.09,10.5L14.77,13.38L15.76,17.66M22,9.24L14.81,8.63L12,2L9.19,8.63L2,9.24L7.45,13.97L5.82,21L12,17.27L18.18,21L16.54,13.97L22,9.24Z"/>
                </svg>
                Our Services
            </div>
            <div class="services-list">
                <div class="service-item">
                    <div class="service-icon">
                        <svg viewBox="0 0 24 24"><path d="M20,11H23V13H20V11M1,11H4V13H1V11M13,1V4H11V1H13M4.92,3.5L7.05,5.64L5.63,7.05L3.5,4.93L4.92,3.5M16.95,5.63L19.07,3.5L20.5,4.93L18.37,7.05L16.95,5.63M12,6A6,6 0 0,1 18,12C18,14.22 16.79,16.16 15,17.2V19A1,1 0 0,1 14,20H10A1,1 0 0,1 9,19V17.2C7.21,16.16 6,14.22 6,12A6,6 0 0,1 12,6M12,8A4,4 0 0,0 8,12C8,13.72 8.89,15.22 10.27,16.11L11,16.5V18H13V16.5L13.73,16.11C15.11,15.22 16,13.72 16,12A4,4 0 0,0 12,8Z"/></svg>
                    </div>
                    <div class="service-content">
                        <h3>Tech Consultations</h3>
                        <p>Strategic guidance for digital transformation</p>
                    </div>
                </div>
                <div class="service-item">
                    <div class="service-icon">
                        <svg viewBox="0 0 24 24"><path d="M17,10.5V7A1,1 0 0,0 16,6H4A1,1 0 0,0 3,7V17A1,1 0 0,0 4,18H16A1,1 0 0,0 17,17V13.5L21,17.5V6.5L17,10.5Z"/></svg>
                    </div>
                    <div class="service-content">
                        <h3>Video Production</h3>
                        <p>Cinematic content & aerial videography</p>
                    </div>
                </div>
                <div class="service-item">
                    <div class="service-icon">
                        <svg viewBox="0 0 24 24"><path d="M12,2A10,10 0 0,1 22,12A10,10 0 0,1 12,22A10,10 0 0,1 2,12A10,10 0 0,1 12,2M12,4A8,8 0 0,0 4,12A8,8 0 0,0 12,20A8,8 0 0,0 20,12A8,8 0 0,0 12,4M12,6A6,6 0 0,1 18,12A6,6 0 0,1 12,18A6,6 0 0,1 6,12A6,6 0 0,1 12,6M12,8A4,4 0 0,0 8,12A4,4 0 0,0 12,16A4,4 0 0,0 16,12A4,4 0 0,0 12,8Z"/></svg>
                    </div>
                    <div class="service-content">
                        <h3>Drone Services</h3>
                        <p>FAA-certified aerial mapping & inspection</p>
                    </div>
                <div class="service-item">
                    <div class="service-icon">
                        <svg viewBox="0 0 24 24"><path d="M12,3C16.97,3 21,7.03 21,12C21,16.97 16.97,21 12,21C7.03,21 3,16.97 3,12C3,7.03 7.03,3 12,3M12,5A7,7 0 0,0 5,12A7,7 0 0,0 12,19A7,7 0 0,0 19,12A7,7 0 0,0 12,5M12,10.5A1.5,1.5 0 0,1 13.5,12A1.5,1.5 0 0,1 12,13.5A1.5,1.5 0 0,1 10.5,12A1.5,1.5 0 0,1 12,10.5M7.5,10.5A1.5,1.5 0 0,1 9,12A1.5,1.5 0 0,1 7.5,13.5A1.5,1.5 0 0,1 6,12A1.5,1.5 0 0,1 7.5,10.5M16.5,10.5A1.5,1.5 0 0,1 18,12A1.5,1.5 0 0,1 16.5,13.5A1.5,1.5 0 0,1 15,12A1.5,1.5 0 0,1 16.5,10.5Z"/></svg>
                    </div>
                    <div class="service-content">
                        <h3>Automation</h3>
                        <p>Workflow optimization & custom solutions</p>
                    </div>
                </div>
            </div>
        </div>

        <!-- QR Code Card -->
        <div class="card qr-section">
            <div class="section-title">
                <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor">
                    <path d="M3,11H5V13H3V11M11,5H13V9H11V5M9,11H13V15H11V13H9V11M15,11H17V13H15V11M15,5H17V9H15V5M11,15H13V17H15V15H17V13H15V11H17V9H19V11H21V9H23V11H21V13H23V15H21V17H19V19H17V21H15V19H13V21H11V19H9V21H7V19H5V17H7V15H9V17H11V15M5,5H9V9H5V5Z"/>
                </svg>
                Share This Card
            </div>
            <div class="qr-wrapper">
                <div id="qrcode"></div>
            </div>
            <p class="qr-text">Scan to save or share this digital card</p>
        </div>

        <!-- Footer -->
        <div class="footer">
            <span class="footer-icon">✈️</span> Tap to connect <span class="footer-icon">🚁</span>
        </div>
    </div>

    <div id="toast" class="toast"></div>

    <script>
        // EDIT: Update these variables with your information
        const VCARD_DATA = {
            name: 'AeroDev Solutions',           // YOUR_NAME
            firstName: 'AeroDev',                // YOUR_FIRST_NAME
            lastName: 'Solutions',               // YOUR_LAST_NAME
            org: 'AeroDev Solutions',            // YOUR_COMPANY
            title: 'Tech Consultations | Video | Drone Automation', // YOUR_TITLE
            phone: '+15551234567',               // YOUR_PHONE
            email: 'hello@aerodev.com',          // YOUR_EMAIL
            website: 'https://yourwebsite.com',  // YOUR_WEBSITE
            address: '123 Sky Lane, Cloud City', // YOUR_ADDRESS
        };

        // Generate QR Code
        window.addEventListener('load', function() {
            const currentURL = window.location.href;
            QRCode.toCanvas(currentURL, {
                width: 200,
                margin: 1,
                color: {
                    dark: '#0c4a6e',
                    light: '#ffffff'
                }
            }, function (error, canvas) {
                if (error) console.error(error);
                const qrDiv = document.getElementById('qrcode');
                qrDiv.innerHTML = '';
                qrDiv.appendChild(canvas);
            });
        });

        // vCard Download Function
        function downloadVCard() {
            const vcard = `BEGIN:VCARD
VERSION:3.0
FN:${VCARD_DATA.name}
N:${VCARD_DATA.lastName};${VCARD_DATA.firstName};;;
ORG:${VCARD_DATA.org}
TITLE:${VCARD_DATA.title}
TEL;TYPE=CELL:${VCARD_DATA.phone}
EMAIL:${VCARD_DATA.email}
URL:${VCARD_DATA.website}
ADR:;;${VCARD_DATA.address};;;;
NOTE:Tech Consultations | Video Production | Drone Services | Automation
END:VCARD`;

            const blob = new Blob([vcard], { type: 'text/vcard;charset=utf-8' });
            const url = window.URL.createObjectURL(blob);
            const link = document.createElement('a');
            link.href = url;
            link.download = `${VCARD_DATA.firstName}_${VCARD_DATA.lastName}.vcf`;
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            window.URL.revokeObjectURL(url);
            
            showToast('Contact saved! ✓');
        }

        // Toast notification
        function showToast(message) {
            const toast = document.getElementById('toast');
            toast.textContent = message;
            toast.classList.add('show');
            setTimeout(() => {
                toast.classList.remove('show');
            }, 3000);
        }

        // Smooth scroll enhancement for mobile
        document.querySelectorAll('a[href^="#"]').forEach(anchor => {
            anchor.addEventListener('click', function (e) {
                e.preventDefault();
                const target = document.querySelector(this.getAttribute('href'));
                if (target) {
                    target.scrollIntoView({ behavior: 'smooth' });
                }
            });
        });
    </script>
<script>(function(){document.addEventListener("click",function(e){var a=e.target.closest("[data-product-id]");if(!a)return;e.preventDefault();var pid=a.getAttribute("data-product-id");if(pid)parent.postMessage({type:"ecto-artifact-link-click",productId:pid},"*")})})();</script>
</body>
</html>
