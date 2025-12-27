<!-- Hero Section: Welcome -->
<section class="welcome-hero" style="display: flex; align-items: center; justify-content: flex-start; gap: 48px; margin-bottom: 0; flex-wrap: wrap; background: #fff; border-radius: 18px 18px 0 0; box-shadow: 0 2px 16px 0 rgba(99,102,241,0.04); padding: 44px 36px 36px 36px;">
    <!-- Left: Welcome Text -->
    <div style="flex: 1 1 420px; min-width: 260px; text-align: left;">
        <h1 style="font-size: 2.8rem; font-weight: 800; margin-bottom: 0; line-height: 1.08; color: #23272f; letter-spacing: 0.5px;">
            Welcome to<br>
            <span style="display: block; padding-left: 90px; font-size: 3.3rem; font-weight: 900; color: #6366f1; margin-top: 0.1em; letter-spacing: 1.5px; text-shadow: 0 2px 12px #e0e7ff;">HiT Blog</span>
        </h1>
        <h2 style="font-size: 1.25rem; font-weight: 600; margin: 22px 0 16px 0; color: #374151; letter-spacing: 0.2px;">Nơi chia sẻ kiến thức lập trình</h2>
        <p style="font-size: 1.09rem; max-width: 500px; line-height: 1.7; margin-bottom: 0; color: #444; border-left: 3px solid #6366f1; padding-left: 16px; background: #f8fafc; border-radius: 6px;">
            Góc nhìn kỹ thuật về lập trình, từ nền tảng đến cách áp dụng trong các bài toán thực tế.
        </p>
    </div>
    <!-- Right: Animated Dev SVG -->
    <div style="flex: 1 1 33%; min-width: 260px; max-width: 420px; display: flex; align-items: center; justify-content: center;">
        <!-- Modern Dev Illustration SVG: Desk, Monitor, Plant, Lamp, Animated Code -->
        <svg width="320" height="220" viewBox="0 0 320 220" fill="none" xmlns="http://www.w3.org/2000/svg" style="display: block;">
            <!-- Desk -->
            <rect x="40" y="180" width="240" height="18" rx="8" fill="#6366f1"/>
            <!-- Monitor -->
            <rect x="110" y="80" width="100" height="60" rx="10" fill="#f1f5f9" stroke="#6366f1" stroke-width="3"/>
            <!-- Monitor Stand -->
            <rect x="150" y="140" width="20" height="18" rx="6" fill="#818cf8"/>
            <!-- Keyboard -->
            <rect x="120" y="165" width="80" height="10" rx="4" fill="#a5b4fc"/>
            <!-- Chair -->
            <ellipse cx="160" cy="200" rx="32" ry="10" fill="#818cf8" opacity="0.18"/>
            <!-- Dev Body -->
            <rect x="145" y="60" width="30" height="38" rx="14" fill="#6366f1">
                <animate attributeName="y" values="60;56;60" dur="2.2s" repeatCount="indefinite"/>
            </rect>
            <!-- Dev Head -->
            <circle cx="160" cy="48" r="18" fill="#fbbf24">
                <animate attributeName="cy" values="48;44;48" dur="2.2s" repeatCount="indefinite"/>
            </circle>
            <!-- Left Arm -->
            <rect x="128" y="80" width="16" height="36" rx="7" fill="#818cf8">
                <animate attributeName="y" values="80;76;80" dur="2.2s" repeatCount="indefinite"/>
            </rect>
            <!-- Right Arm -->
            <rect x="176" y="80" width="16" height="36" rx="7" fill="#818cf8">
                <animate attributeName="y" values="80;76;80" dur="2.2s" repeatCount="indefinite"/>
            </rect>
            <!-- Plant Pot -->
            <rect x="60" y="160" width="18" height="18" rx="5" fill="#fbbf24"/>
            <!-- Plant Leaves -->
            <ellipse cx="69" cy="155" rx="9" ry="14" fill="#34d399"/>
            <ellipse cx="78" cy="158" rx="6" ry="10" fill="#10b981"/>
            <!-- Lamp Base -->
            <rect x="242" y="150" width="10" height="30" rx="4" fill="#818cf8"/>
            <!-- Lamp Head -->
            <ellipse cx="247" cy="145" rx="12" ry="7" fill="#fbbf24"/>
            <!-- Animated Code Lines -->
            <rect x="120" y="95" width="60" height="8" rx="3" fill="#a5b4fc">
                <animate attributeName="width" values="60;90;60" dur="2.2s" repeatCount="indefinite"/>
            </rect>
            <rect x="120" y="110" width="80" height="8" rx="3" fill="#818cf8">
                <animate attributeName="width" values="80;60;80" dur="2.2s" repeatCount="indefinite"/>
            </rect>
            <rect x="120" y="125" width="50" height="8" rx="3" fill="#6366f1">
                <animate attributeName="width" values="50;70;50" dur="2.2s" repeatCount="indefinite"/>
            </rect>
        </svg>
    </div>
</section>
<hr style="margin: 0; border: none; border-top: 3px solid #ef4444; border-radius: 2px; width: 100%; max-width: 1000px;" />

<!-- Main Content Section -->
<section class="main-content-sections" style="display: flex; gap: 40px; margin: 0 0 56px 0; align-items: flex-start; flex-wrap: wrap; background: #f8fafc; border-radius: 0 0 18px 18px; box-shadow: 0 2px 16px 0 rgba(99,102,241,0.04); padding: 36px 36px 44px 36px; border-top: none;">
    <style>
    header .site-title a {
        font-size: 2.3rem !important;
        font-weight: 900 !important;
        letter-spacing: 1.5px;
    }
    header {
        transition: transform 0.36s cubic-bezier(.4,1.6,.6,1), box-shadow 0.36s cubic-bezier(.4,1.6,.6,1), height 0.36s, padding 0.36s;
        will-change: transform, box-shadow;
    }
    header.home-sticky {
        position: fixed !important;
        top: 0;
        left: 0;
        width: 100vw;
        z-index: 1000;
        background: #fff;
        box-shadow: 0 6px 32px 0 rgba(99,102,241,0.13);
        transform: scaleY(0.75);
        transform-origin: top center;
        height: 65px !important;
        padding-top: 0 !important;
        padding-bottom: 0 !important;
    }
    @media (max-width: 968px) {
        header.home-sticky {
            height: 54px !important;
        }
    }
    </style>
    <script>
        (function() {
            var last = false;
            var header = document.querySelector('header');
            if (!header) return;
            function onScroll() {
                if (window.scrollY > 40) {
                    if (!last) {
                        header.classList.add('home-sticky');
                        last = true;
                    }
                } else {
                    if (last) {
                        header.classList.remove('home-sticky');
                        last = false;
                    }
                }
            }
            window.addEventListener('scroll', onScroll, { passive: true });
            // Nếu đã ở dưới khi load lại trang
            onScroll();
        })();
    </script>
    <!-- Left: Articles and Tutorials -->
    <div style="flex: 2 1 400px; min-width: 320px;">
        <h2 style="color: #e11d48; letter-spacing: 2px; font-size: 1.1rem; font-weight: 700; margin-bottom: 18px; text-transform: uppercase;">Articles and Tutorials</h2>
        <!-- Article 1 -->
        <div style="margin-bottom: 36px;">
            <h3 style="font-size: 1.45rem; font-weight: 700; margin-bottom: 8px;">Java Collections Framework</h3>
            <div style="color: #6b7280; font-size: 1.05rem; margin-bottom: 8px;">Tìm hiểu về các cấu trúc dữ liệu và API Collections trong Java, cách sử dụng List, Set, Map hiệu quả.</div>
            <a href="/blog/posts/java-collections-framework/" style="font-weight: 600; color: #2563eb; text-decoration: none;">Read more</a>
        </div>
        <!-- Article 2 -->
        <div style="margin-bottom: 36px;">
            <h3 style="font-size: 1.45rem; font-weight: 700; margin-bottom: 8px;">Java Exception Handling</h3>
            <div style="color: #6b7280; font-size: 1.05rem; margin-bottom: 8px;">Cách xử lý ngoại lệ trong Java, phân biệt checked/unchecked exception và best practices.</div>
            <a href="/blog/posts/java-exception-handling/" style="font-weight: 600; color: #2563eb; text-decoration: none;">Read more</a>
        </div>
        <!-- Article 3 -->
        <div style="margin-bottom: 36px;">
            <h3 style="font-size: 1.45rem; font-weight: 700; margin-bottom: 8px;">JavaScript Async/Await</h3>
            <div style="color: #6b7280; font-size: 1.05rem; margin-bottom: 8px;">Giải thích về bất đồng bộ trong JavaScript, cách sử dụng async/await và promise.</div>
            <a href="/blog/posts/javascript-async-await/" style="font-weight: 600; color: #2563eb; text-decoration: none;">Read more</a>
        </div>
        <!-- Article 4 -->
        <div style="margin-bottom: 36px;">
            <h3 style="font-size: 1.45rem; font-weight: 700; margin-bottom: 8px;">Java Multithreading</h3>
            <div style="color: #6b7280; font-size: 1.05rem; margin-bottom: 8px;">Lập trình đa luồng trong Java, thread, runnable, đồng bộ hóa và các vấn đề concurrency.</div>
            <a href="/blog/posts/java-multithreading/" style="font-weight: 600; color: #2563eb; text-decoration: none;">Read more</a>
        </div>
        <!-- Article 5 -->
        <div style="margin-bottom: 36px;">
            <h3 style="font-size: 1.45rem; font-weight: 700; margin-bottom: 8px;">JavaScript DOM Manipulation</h3>
            <div style="color: #6b7280; font-size: 1.05rem; margin-bottom: 8px;">Cách thao tác DOM bằng JavaScript hiện đại, ví dụ thực tế và best practices.</div>
            <a href="/blog/posts/javascript-dom-manipulation/" style="font-weight: 600; color: #2563eb; text-decoration: none;">Read more</a>
        </div>
        <div style="text-align: left; margin-top: 8px;">
            <a href="/blog/posts/" style="display: inline-block; background: #6366f1; color: #fff; border: none; border-radius: 16px; padding: 8px 22px; font-size: 1.08rem; font-weight: 700; text-decoration: none; letter-spacing: 1px; transition: background 0.2s;">All posts</a>
        </div>
    </div>
    <!-- Right: Sidebar -->
    <div style="flex: 1 1 260px; min-width: 220px; max-width: 320px;">
        <div style="margin-bottom: 36px;">
            <h3 style="color: #e11d48; font-size: 1.1rem; font-weight: 700; margin-bottom: 14px; text-transform: uppercase; letter-spacing: 1px;">Browse By Category</h3>
            <div style="display: flex; gap: 10px; flex-wrap: wrap;">
                <a href="/blog/tags/java/" style="background: #e0e7ff; color: #3730a3; font-weight: 600; padding: 6px 18px; border-radius: 18px; text-decoration: none; font-size: 1rem;">Java</a>
                <a href="/blog/tags/javascript/" style="background: #fef9c3; color: #b45309; font-weight: 600; padding: 6px 18px; border-radius: 18px; text-decoration: none; font-size: 1rem;">JavaScript</a>
            </div>
        </div>
        <div>
            <h3 style="color: #e11d48; font-size: 1.1rem; font-weight: 700; margin-bottom: 14px; text-transform: uppercase; letter-spacing: 1px;">Popular Content</h3>
            <ul id="popular-list" style="list-style: none; padding: 0; margin: 0;">
                <li style="margin-bottom: 12px;"><a href="/blog/posts/java-collections-framework/" style="color: #2563eb; text-decoration: none; font-weight: 500;">→ Java Collections Framework</a></li>
                <li style="margin-bottom: 12px;"><a href="/blog/posts/java-exception-handling/" style="color: #2563eb; text-decoration: none; font-weight: 500;">→ Java Exception Handling</a></li>
                <li style="margin-bottom: 12px;"><a href="/blog/posts/javascript-async-await/" style="color: #2563eb; text-decoration: none; font-weight: 500;">→ JavaScript Async/Await</a></li>
                <li style="margin-bottom: 12px;"><a href="/blog/posts/java-multithreading/" style="color: #2563eb; text-decoration: none; font-weight: 500;">→ Java Multithreading</a></li>
                <li style="margin-bottom: 12px;"><a href="/blog/posts/javascript-dom-manipulation/" style="color: #2563eb; text-decoration: none; font-weight: 500;">→ JavaScript DOM Manipulation</a></li>
                <li style="margin-bottom: 12px; display: none;"><a href="/blog/posts/javascript-basics/" style="color: #2563eb; text-decoration: none; font-weight: 500;">→ JavaScript Basics</a></li>
                <li style="margin-bottom: 12px; display: none;"><a href="/blog/posts/javascript-functions/" style="color: #2563eb; text-decoration: none; font-weight: 500;">→ JavaScript Functions</a></li>
                <li style="margin-bottom: 12px; display: none;"><a href="/blog/posts/java-oop-basics/" style="color: #2563eb; text-decoration: none; font-weight: 500;">→ Java OOP Basics</a></li>
                <li style="margin-bottom: 12px; display: none;"><a href="/blog/posts/java-socket-programming/" style="color: #2563eb; text-decoration: none; font-weight: 500;">→ Java Socket Programming</a></li>
            </ul>
            <button id="popular-toggle-btn" style="margin-top: 8px; background: #6366f1; color: #fff; border: none; border-radius: 16px; padding: 8px 22px; font-size: 1.08rem; font-weight: 700; cursor: pointer; text-decoration: none; letter-spacing: 1px; transition: background 0.2s;">Show more</button>
            <script>
            (function() {
                var btn = document.getElementById('popular-toggle-btn');
                var list = document.getElementById('popular-list');
                var items = list.querySelectorAll('li');
                var initial = 5;
                var step = 3;
                var shown = initial;
                function update() {
                    for (var i = 0; i < items.length; ++i) {
                        if (i < shown) {
                            items[i].style.display = '';
                        } else {
                            items[i].style.display = 'none';
                        }
                    }
                    if (shown >= items.length) {
                        btn.textContent = 'Show less';
                    } else {
                        btn.textContent = 'Show more';
                    }
                }
                btn.addEventListener('click', function() {
                    if (shown >= items.length) {
                        shown = initial;
                    } else {
                        shown = Math.min(shown + step, items.length);
                    }
                    update();
                });
                update();
            })();
            </script>
        </div>
    </div>

</section>


