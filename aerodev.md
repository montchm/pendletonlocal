<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=5.0">
    <title>Your Name | Aeronautical Tech Contact</title>
    <meta name="description" content="Mitchell Montchalin - The Drone Enthusiast">
    
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;600;700&family=Outfit:wght@300;400;500;600&display=swap" rel="stylesheet">
    <script src="https://unpkg.com/lucide@latest/dist/umd/lucide.js"></script>
    
    <style>
        :root {
            --bg-primary: #020617;
            --bg-secondary: #0f172a;
            --accent-blue: #38bdf8;
            --accent-cyan: #22d3ee;
            --accent-violet: #8b5cf6;
            --text-primary: #f8fafc;
            --text-secondary: #94a3b8;
            --glass-bg: rgba(255, 255, 255, 0.05);
            --glass-border: rgba(255, 255, 255, 0.1);
        }

        * {
            -webkit-tap-highlight-color: transparent;
        }

        body {
            font-family: 'Outfit', sans-serif;
            background: var(--bg-primary);
            color: var(--text-primary);
            overflow-x: hidden;
            min-height: 100vh;
        }

        h1, h2, h3 {
            font-family: 'Space Grotesk', sans-serif;
        }

        .sky-gradient {
            background: radial-gradient(ellipse at top center, #1e3a8a 0%, #0f172a 50%, #020617 100%);
            position: relative;
        }

        .sky-gradient::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: 
                radial-gradient(circle at 20% 20%, rgba(56, 189, 248, 0.12) 0%, transparent 40%),
                radial-gradient(circle at 80% 60%, rgba(139, 92, 246, 0.08) 0%, transparent 40%);
            pointer-events: none;
        }

        .glass-card {
            background: var(--glass-bg);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border: 1px solid var(--glass-border);
            box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.4);
        }

        @keyframes fly-across {
            0% { transform: translateX(-100px); opacity: 0; }
            10% { opacity: 0.3; }
            90% { opacity: 0.3; }
            100% { transform: translateX(calc(100vw + 100px)); opacity: 0; }
        }

        @keyframes float {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-15px); }
        }

        @keyframes pulse-ring {
            0% { transform: scale(0.95); opacity: 1; }
            100% { transform: scale(1.5); opacity: 0; }
        }

        .floating {
            animation: float 4s ease-in-out infinite;
        }

        .plane-fly {
            animation: fly-across 20s linear infinite;
        }

        .drone-fly {
            animation: fly-across 25s linear infinite 8s;
        }

        .gradient-text {
            background: linear-gradient(135deg, var(--accent-blue), var(--accent-cyan), var(--accent-violet));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .btn-contact {
            min-height: 56px;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            position: relative;
            overflow: hidden;
        }

        .btn-contact:active {
            transform: scale(0.97);
        }

        .btn-contact::before {
            content: '';
            position: absolute;
            top: 50%;
            left: 50%;
            width: 0;
            height: 0;
            border-radius: 50%;
            background: rgba(255,255,255,0.2);
            transform: translate(-50%, -50%);
            transition: width 0.6s, height 0.6s;
        }

        .btn-contact:active::before {
            width: 300px;
            height: 300px;
        }

        .social-link {
            min-width: 56px;
            min-height: 56px;
            transition: all 0.3s ease;
        }

        .social-link:active {
            transform: scale(0.95);
        }

        .status-ring {
            position: absolute;
            width: 100%;
            height: 100%;
            border-radius: 50%;
            border: 2px solid var(--accent-blue);
            animation: pulse-ring 2s cubic-bezier(0, 0, 0.2, 1) infinite;
        }

        input:focus {
            outline: none;
            border-color: var(--accent-blue);
            box-shadow: 0 0 0 3px rgba(56, 189, 248, 0.1);
        }
    </style>
</head>
<body class="antialiased">
    
    <!-- Animated Background Elements -->
    <div class="fixed inset-0 overflow-hidden pointer-events-none z-0">
        <div class="plane-fly absolute top-[10%]">
            <svg width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="rgba(56, 189, 248, 0.3)" stroke-width="1.5">
                <path d="M17.8 19.2 16 11l3.5-3.5C21 6 21.5 4 21 3c-1-.5-3 0-4.5 1.5L13 8 4.8 6.2c-.5-.1-.9.1-1.1.5l-.3.5c-.2.5-.1 1 .3 1.3L9 11l-2 2.5L5 16l-.3 1c0 .5.3.9.8 1l1 .3 2.5-2 2.5 5.5c.3.4.8.5 1.3.3l.5-.3c.4-.2.6-.6.5-1.1L11 13l8.2 5.8c.5.3 1.2.4 1.6-.1l.3-.5c.2-.4.1-.9-.3-1.1z"/>
            </svg>
        </div>
        <div class="drone-fly absolute top-[30%]">
            <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="rgba(139, 92, 246, 0.3)" stroke-width="1.5">
                <path d="M3 12h18M12 3v18M8 8l8 8M16 8l-8 8"/>
                <circle cx="12" cy="12" r="3"/>
                <circle cx="6" cy="6" r="1.5"/>
                <circle cx="18" cy="6" r="1.5"/>
                <circle cx="6" cy="18" r="1.5"/>
                <circle cx="18" cy="18" r="1.5"/>
            </svg>
        </div>
    </div>

    <!-- Main Container -->
    <main class="sky-gradient min-h-screen flex items-center justify-center p-4 sm:p-6 relative z-10">
        <div class="w-full max-w-md mx-auto">
            
            <!-- Profile Card -->
            <div class="glass-card rounded-3xl p-6 sm:p-8 mb-6">
                <!-- Avatar with Status -->
                <div class="flex flex-col items-center text-center mb-6">
                    <div class="relative mb-4 floating">
                        <div class="w-28 h-28 sm:w-32 sm:h-32 rounded-full bg-gradient-to-br from-sky-400 via-cyan-400 to-violet-500 flex items-center justify-center relative">
                            <div class="status-ring"></div>
                            <i data-lucide="plane" class="w-12 h-12 sm:w-14 sm:h-14 text-white relative z-10"></i>
                        </div>
                        <div class="absolute -bottom-1 -right-1 w-8 h-8 bg-emerald-500 rounded-full border-4 border-slate-900 flex items-center justify-center">
                            <i data-lucide="check" class="w-4 h-4 text-white"></i>
                        </div>
                    </div>
                    
                    <!-- EDIT: Your Name -->
                    <h1 class="text-2xl sm:text-3xl font-bold mb-2">Mitchell Montchalin</h1>
                    
                    <!-- EDIT: Your Title -->
                    <p class="text-sm sm:text-base gradient-text font-semibold mb-3">
                        Technology consultant
                    </p>
                    
                    <!-- EDIT: Your Tagline -->
                    <p class="text-sm text-slate-400 leading-relaxed px-4">
                        Here to Hype you up, because the sky is the limit.
                    </p>

                    <!-- Quick Stats -->
                    <div class="flex gap-6 mt-5 pt-5 border-t border-white/10">
                        <div class="text-center">
                            <p class="text-xl font-bold gradient-text">35+</p>
                            <p class="text-xs text-slate-500">Years</p>
                        </div>
                        <div class="text-center">
                            <p class="text-xl font-bold gradient-text">Endlessly</p>
                            <p class="text-xs text-slate-500">Curious</p>
                        </div>
                        <div class="text-center">
                            <p class="text-xl font-bold gradient-text">Colemak</p>
                            <p class="text-xs text-slate-500">Certified</p>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Contact Actions -->
            <div class="space-y-3 mb-6">
                <!-- EDIT: Phone Number -->
                <a href="tel:+15551234567" class="btn-contact glass-card rounded-2xl p-4 flex items-center gap-4 active:bg-sky-500/20">
                    <div class="w-12 h-12 rounded-xl bg-sky-500/20 flex items-center justify-center flex-shrink-0">
                        <i data-lucide="phone" class="w-6 h-6 text-sky-400"></i>
                    </div>
                    <div class="flex-1 text-left">
                        <p class="text-xs text-slate-500 mb-0.5">Call Me</p>
                        <p class="font-semibold">+1 541 304 9769</p>
                    </div>
                    <i data-lucide="chevron-right" class="w-5 h-5 text-slate-600"></i>
                </a>

                <!-- EDIT: Email -->
                <a href="mailto:alex@aerotech.com" class="btn-contact glass-card rounded-2xl p-4 flex items-center gap-4 active:bg-violet-500/20">
                    <div class="w-12 h-12 rounded-xl bg-violet-500/20 flex items-center justify-center flex-shrink-0">
                        <i data-lucide="mail" class="w-6 h-6 text-violet-400"></i>
                    </div>
                    <div class="flex-1 text-left">
                        <p class="text-xs text-slate-500 mb-0.5">Email Me</p>
                        <p class="font-semibold truncate">mitchm32@hotmail.com</p>
                    </div>
                    <i data-lucide="chevron-right" class="w-5 h-5 text-slate-600"></i>
                </a>

                <!-- EDIT: WhatsApp -->
                <a href="https://wa.me/15551234567" class="btn-contact glass-card rounded-2xl p-4 flex items-center gap-4 active:bg-emerald-500/20">
                    <div class="w-12 h-12 rounded-xl bg-emerald-500/20 flex items-center justify-center flex-shrink-0">
                        <i data-lucide="message-circle" class="w-6 h-6 text-emerald-400"></i>
                    </div>
                    <div class="flex-1 text-left">
                        <p class="text-xs text-slate-500 mb-0.5">WhatsApp</p>
                        <p class="font-semibold">Chat Directly</p>
                    </div>
                    <i data-lucide="chevron-right" class="w-5 h-5 text-slate-600"></i>
                </a>

                <!-- Save Contact Button -->
                <button id="saveContact" class="btn-contact w-full glass-card rounded-2xl p-4 flex items-center justify-center gap-3 bg-gradient-to-r from-sky-500 to-cyan-500 active:opacity-90">
                    <i data-lucide="user-plus" class="w-6 h-6 text-white"></i>
                    <span class="font-semibold text-white">Save to Contacts</span>
                </button>
            </div>

            <!-- Social Media Links -->
            <div class="glass-card rounded-3xl p-6">
                <p class="text-sm font-semibold text-slate-400 mb-4 text-center">Connect on Social</p>
                <div class="grid grid-cols-4 gap-3">
                    <!-- EDIT: LinkedIn URL -->
                    <a href="https://linkedin.com/in/yourprofile" class="social-link glass-card rounded-xl flex items-center justify-center active:bg-sky-500/20" aria-label="LinkedIn">
                        <i data-lucide="linkedin" class="w-6 h-6 text-sky-400"></i>
                    </a>
                    
                    <!-- EDIT: Instagram URL -->
                    <a href="https://instagram.com/hundredairesociety" class="social-link glass-card rounded-xl flex items-center justify-center active:bg-pink-500/20" aria-label="Instagram">
                        <i data-lucide="instagram" class="w-6 h-6 text-pink-400"></i>
                    </a>
                    
                    <!-- EDIT: Twitter/X URL -->
                    <a href="https://twitter.com/hundredairesociety" class="social-link glass-card rounded-xl flex items-center justify-center active:bg-slate-500/20" aria-label="Twitter">
                        <i data-lucide="twitter" class="w-6 h-6 text-slate-300"></i>
                    </a>
                    
                    <!-- EDIT: YouTube URL -->
                    <a href="https://youtube.com/hundredairesociety" class="social-link glass-card rounded-xl flex items-center justify-center active:bg-red-500/20" aria-label="YouTube">
                        <i data-lucide="youtube" class="w-6 h-6 text-red-400"></i>
                    </a>
                    
                    <!-- EDIT: GitHub URL -->
                    <a href="https://github.com/montchm" class="social-link glass-card rounded-xl flex items-center justify-center active:bg-slate-500/20" aria-label="GitHub">
                        <i data-lucide="github" class="w-6 h-6 text-slate-300"></i>
                    </a>
                    
                    <!-- EDIT: Website URL -->
                    <a href="https://yourwebsite.com" class="social-link glass-card rounded-xl flex items-center justify-center active:bg-violet-500/20" aria-label="Website">
                        <i data-lucide="globe" class="w-6 h-6 text-violet-400"></i>
                    </a>
                    
                    <!-- EDIT: Drone Portfolio URL -->
                    <a href="https://yourportfoliodrones.com" class="social-link glass-card rounded-xl flex items-center justify-center active:bg-cyan-500/20" aria-label="Drone Portfolio">
                        <i data-lucide="video" class="w-6 h-6 text-cyan-400"></i>
                    </a>
                    
                    <!-- EDIT: Calendar/Booking URL -->
                    <a href="https://calendly.com/yourprofile" class="social-link glass-card rounded-xl flex items-center justify-center active:bg-emerald-500/20" aria-label="Book Meeting">
                        <i data-lucide="calendar" class="w-6 h-6 text-emerald-400"></i>
                    </a>
                </div>
            </div>

            <!-- Footer Note -->
            <p class="text-xs text-slate-600 text-center mt-6">
                © 2024 • Available for worldwide projects
            </p>
        </div>
    </main>

    <!-- Hidden vCard Data for Save Contact -->
    <div id="vcard-data" class="hidden">
        BEGIN:VCARD
        VERSION:3.0
        FN:Mitchell Montchalin
        TITLE:Technology Consultant
        TEL;TYPE=CELL:+15413049769
        EMAIL:montchm@eou.edu
        URL:https://montchm.github.io/pendletonlocal
        END:VCARD
    </div>

    <script>
        lucide.createIcons();

        // Save Contact Function
        document.getElementById('saveContact').addEventListener('click', function() {
            // EDIT: Update vCard details here
            const vCardData = `BEGIN:VCARD
VERSION:3.0
FN:Mitchell Montchalin
TITLE:Technology Consultant
TEL;TYPE=CELL:+15413049769
EMAIL:mitchm32@hotmail.com
URL:https://montchm.github.io/pendletonlocal
END:VCARD`;

            const blob = new Blob([vCardData], { type: 'text/vcard' });
            const url = window.URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = 'mitchell-montchalin-contact.vcf';
            document.body.appendChild(a);
            a.click();
            window.URL.revokeObjectURL(url);
            document.body.removeChild(a);

            // Visual feedback
            const btn = this;
            const originalHTML = btn.innerHTML;
            btn.innerHTML = '<i data-lucide="check" class="w-6 h-6 text-white"></i><span class="font-semibold text-white">Saved!</span>';
            lucide.createIcons();
            
            setTimeout(() => {
                btn.innerHTML = originalHTML;
                lucide.createIcons();
            }, 2000);
        });

        // Prevent horizontal scroll on mobile
        document.addEventListener('touchmove', function(e) {
            if (e.scale !== 1) { e.preventDefault(); }
        }, { passive: false });
    </script>
<script>(function(){document.addEventListener("click",function(e){var a=e.target.closest("[data-product-id]");if(!a)return;e.preventDefault();var pid=a.getAttribute("data-product-id");if(pid)parent.postMessage({type:"ecto-artifact-link-click",productId:pid},"*")})})();</script>
</body>
</html>
