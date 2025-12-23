# Ex.08 Design of Interactive Image Gallery
## Date:13/12/2025

## AIM:
To design a web application for an inteactive image gallery with minimum five images.

## DESIGN STEPS:

### Step 1:
Clone the github repository and create Django admin interface.

### Step 2:
Change settings.py file to allow request from all hosts.

### Step 3:
Use CSS for positioning and styling.

### Step 4:
Write JavaScript program for implementing interactivity.

### Step 5:
Validate the HTML and CSS code.

### Step 6:
Publish the website in the given URL.

## PROGRAM :
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SUPRA - Luxury Gallery</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@400;600;700&family=Playfair+Display:wght@400;500;600&display=swap');
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Playfair Display', serif;
            background: linear-gradient(135deg, #0f0f23 0%, #1a1a2e 50%, #16213e 100%);
            color: #ffffff;
            overflow-x: hidden;
            min-height: 100vh;
        }

        header {
            text-align: center;
            background: linear-gradient(45deg, rgba(255,215,0,0.1), rgba(218,165,32,0.2));
            backdrop-filter: blur(20px);
            border-bottom: 1px solid rgba(255,215,0,0.3);
            padding: 2rem 0;
            position: relative;
        }

        header::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 10"><defs><linearGradient id="g" x1="0%" y1="0%" x2="100%" y2="0%"><stop offset="0%" stop-color="%23FFD700"/><stop offset="50%" stop-color="%23DAA520"/><stop offset="100%" stop-color="%23B8860B"/></linearGradient></defs><rect fill="url(%23g)" height="1" width="100%" opacity="0.6"/><animate attributeName="height" values="1;3;1" dur="3s" repeatCount="indefinite"/></svg>');
            opacity: 0.5;
        }

        h1 {
            font-family: 'Cinzel', serif;
            font-size: clamp(2.5rem, 5vw, 4rem);
            font-weight: 700;
            letter-spacing: 0.1em;
            text-shadow: 0 0 30px rgba(255,215,0,0.8);
            position: relative;
            z-index: 1;
        }

        .gallery-container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 4rem 2rem;
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 2rem;
            position: relative;
        }

        .gallery-item {
            position: relative;
            aspect-ratio: 4/3;
            border-radius: 20px;
            overflow: hidden;
            cursor: pointer;
            transition: all 0.6s cubic-bezier(0.23, 1, 0.320, 1);
            box-shadow: 0 20px 40px rgba(0,0,0,0.4);
            backdrop-filter: blur(10px);
        }

        .gallery-item::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: linear-gradient(45deg, transparent, rgba(255,215,0,0.1), transparent);
            opacity: 0;
            transition: opacity 0.4s ease;
            z-index: 2;
        }

        .gallery-item img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            transition: all 0.6s cubic-bezier(0.23, 1, 0.320, 1);
            filter: grayscale(20%) brightness(0.9);
        }

        .gallery-item:hover {
            transform: translateY(-20px) scale(1.02);
            box-shadow: 0 40px 80px rgba(255,215,0,0.3), 0 0 60px rgba(255,215,0,0.2);
        }

        .gallery-item:hover::before {
            opacity: 1;
            animation: shimmer 2s infinite;
        }

        .gallery-item:hover img {
            filter: grayscale(0%) brightness(1.1);
            transform: scale(1.1);
        }

        @keyframes shimmer {
            0% { transform: translateX(-100%); }
            100% { transform: translateX(100%); }
        }

        /* Modal Styles */
        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.95);
            backdrop-filter: blur(20px);
            animation: fadeIn 0.4s ease-out;
        }

        .modal-content {
            position: relative;
            max-width: 90%;
            max-height: 90%;
            margin: auto;
            top: 50%;
            transform: translateY(-50%);
            border-radius: 20px;
            overflow: hidden;
            box-shadow: 0 50px 100px rgba(0,0,0,0.8);
        }

        #modalImage {
            width: 100%;
            height: auto;
            display: block;
        }

        .close {
            position: absolute;
            top: -50px;
            right: 0;
            color: #FFD700;
            font-size: 3rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            text-shadow: 0 0 20px rgba(255,215,0,0.8);
        }

        .close:hover {
            transform: scale(1.2) rotate(90deg);
            color: #ffffff;
            filter: drop-shadow(0 0 10px #FFD700);
        }

        .nav-arrows {
            position: absolute;
            top: 50%;
            transform: translateY(-50%);
            background: rgba(255,215,0,0.2);
            border: none;
            color: #FFD700;
            font-size: 2rem;
            padding: 1rem 1.5rem;
            cursor: pointer;
            border-radius: 50%;
            transition: all 0.3s ease;
            backdrop-filter: blur(10px);
            width: 60px;
            height: 60px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .nav-prev { left: 20px; }
        .nav-next { right: 20px; }

        .nav-arrows:hover {
            background: rgba(255,215,0,0.4);
            transform: translateY(-50%) scale(1.1);
            box-shadow: 0 10px 30px rgba(255,215,0,0.4);
        }

        /* Responsive */
        @media (max-width: 768px) {
            .gallery-container {
                grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
                gap: 1.5rem;
                padding: 2rem 1rem;
            }
            
            .close {
                top: -30px;
                font-size: 2rem;
            }
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        /* Loading animation */
        .loading {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, #0f0f23, #1a1a2e);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 2000;
            transition: opacity 0.5s ease;
        }

        .spinner {
            width: 50px;
            height: 50px;
            border: 3px solid rgba(255,215,0,0.3);
            border-top: 3px solid #FFD700;
            border-radius: 50%;
            animation: spin 1s linear infinite;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
    </style>
</head>
<body>
    <div class="loading" id="loading">
        <div class="spinner"></div>
    </div>

    <header>
        <h1>SUPRA</h1>
    </header>

    <div class="gallery-container">
        <div class="gallery-item" onclick="openModal(this)">
            <img src="img1.jpg" alt="Supra Collection 1" loading="lazy">
        </div>
        <div class="gallery-item" onclick="openModal(this)">
            <img src="img2.jpg" alt="Supra Collection 2" loading="lazy">
        </div>
        <div class="gallery-item" onclick="openModal(this)">
            <img src="img3.jpg" alt="Supra Collection 3" loading="lazy">
        </div>
        <div class="gallery-item" onclick="openModal(this)">
            <img src="img4.jpg" alt="Supra Collection 4" loading="lazy">
        </div>
    </div>

    <div id="modal" class="modal">
        <span class="close" onclick="closeModal()">&times;</span>
        <button class="nav-arrows nav-prev" onclick="changeImage(-1)">&#10094;</button>
        <img id="modalImage" class="modal-content">
        <button class="nav-arrows nav-next" onclick="changeImage(1)">&#10095;</button>
    </div>

    <script>
        // Gallery functionality
        let currentImageIndex = 0;
        const galleryItems = document.querySelectorAll('.gallery-item');

        function openModal(element) {
            const modal = document.getElementById('modal');
            const modalImg = document.getElementById('modalImage');
            currentImageIndex = Array.from(galleryItems).indexOf(element);
            
            modal.style.display = 'block';
            modalImg.src = galleryItems[currentImageIndex].querySelector('img').src;
            document.body.style.overflow = 'hidden';
        }

        function closeModal() {
            document.getElementById('modal').style.display = 'none';
            document.body.style.overflow = 'auto';
        }

        function changeImage(direction) {
            currentImageIndex = (currentImageIndex + direction + galleryItems.length) % galleryItems.length;
            document.getElementById('modalImage').src = galleryItems[currentImageIndex].querySelector('img').src;
        }

        // Close modal on outside click
        window.onclick = function(event) {
            const modal = document.getElementById('modal');
            if (event.target === modal) {
                closeModal();
            }
        }

        // Keyboard navigation
        document.addEventListener('keydown', function(e) {
            const modal = document.getElementById('modal');
            if (modal.style.display === 'block') {
                if (e.key === 'Escape') closeModal();
                if (e.key === 'ArrowLeft') changeImage(-1);
                if (e.key === 'ArrowRight') changeImage(1);
            }
        });

        // Hide loading screen after images load
        window.addEventListener('load', function() {
            setTimeout(() => {
                document.getElementById('loading').style.opacity = '0';
                setTimeout(() => {
                    document.getElementById('loading').style.display = 'none';
                }, 500);
            }, 1000);
        });

        // Smooth scrolling and performance optimizations
        galleryItems.forEach((item, index) => {
            item.style.opacity = '0';
            item.style.transform = 'translateY(50px)';
            
            const observer = new IntersectionObserver((entries) => {
                entries.forEach(entry => {
                    if (entry.isIntersecting) {
                        setTimeout(() => {
                            entry.target.style.transition = 'opacity 0.8s ease, transform 0.8s ease';
                            entry.target.style.opacity = '1';
                            entry.target.style.transform = 'translateY(0)';
                        }, index * 100);
                    }
                });
            });
            observer.observe(item);
        });
    </script>
</body>
</html>
```
## OUTPUT:

<img width="1866" height="870" alt="Screenshot 2025-12-17 162755" src="https://github.com/user-attachments/assets/3569537a-5c8b-4cbd-9a58-559bb407c174" />

## RESULT:
The program for designing an interactive image gallery using HTML, CSS and JavaScript is executed successfully.
