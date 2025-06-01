<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Galaxy Particle Demo</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Orbitron&display=swap');

  body, html {
    margin: 0; padding: 0; height: 100%; overflow: hidden;
    background: radial-gradient(ellipse at center, #000014 0%, #000000 100%);
    font-family: 'Orbitron', sans-serif;
    user-select: none;
    -webkit-tap-highlight-color: transparent;
  }

  #galaxy {
    position: fixed;
    top: 50%; left: 50%;
    transform-style: preserve-3d;
    transform-origin: center center;
    pointer-events: none;
    width: 100vw;
    height: 100vh;
    overflow: visible;
    z-index: 0;
  }

  .star {
    position: absolute;
    width: 2px;
    height: 2px;
    background: white;
    border-radius: 50%;
    filter: drop-shadow(0 0 2px white);
    will-change: transform, opacity;
  }

  .text-particle {
    position: absolute;
    white-space: nowrap;
    pointer-events: none;
    font-weight: 700;
    text-shadow: 0 0 70px #fff, 0 0 70px #fff;
    will-change: transform, opacity;
    user-select: none;
  }
  .text-particle.love { color: #ff3399; text-shadow: 0 0 20px #ff3399; }
  .text-particle.birthday { color: #ff3399; text-shadow: 0 0 20px #ff3399; }
  .text-particle.date { color: #ff3399; text-shadow: 0 0 20px #ff3399; }
  .text-particle.special { color: #ff3399; text-shadow: 0 0 20px #ff3399; }
  .text-particle.heart { color: #ff3399; text-shadow: 0 0 20px #ff3399; }

  .text-particle.image-particle {
    pointer-events: auto;
    user-select: none;
    box-shadow: 0 4px 24px rgba(0,0,0,0.3);
    border-radius: 12px;
    background: white;
    width: 80px;
    height: auto;
  }

  #loadingScreen, #errorScreen {
    position: fixed; top: 0; left: 0; width: 100%; height: 100%;
    display: flex; justify-content: center; align-items: center;
    font-size: 24px; color: white; background: rgba(0,0,0,0.7);
    z-index: 9999;
  }

  #errorScreen {
    display: none;
    flex-direction: column;
    gap: 12px;
  }

  #copyrightFooter {
    position: fixed;
    bottom: 10px; right: 10px;
    z-index: 999;
    color: #fff;
    font-size: 13px;
    opacity: 0.7;
    font-family: 'Orbitron', sans-serif;
    user-select: none;
  }
</style>
</head>
<body>

<div id="loadingScreen">Loading Galaxy...</div>
<div id="errorScreen">
  <div>Không thể tải dữ liệu Galaxy!</div>
  <button onclick="location.reload()">Thử lại</button>
</div>

<div id="galaxy"></div>

<audio id="galaxyAudio" src="https://dn720304.ca.archive.org/0/items/anh-nang-cua-anh-duc-phuc-download-lossless-500kbps-320kbps-anh-nang-cua-anh-duc-phuc/%C3%81nh%20N%E1%BA%AFng%20C%E1%BB%A7a%20Anh%20-%20%C4%90%E1%BB%A9c%20Ph%C3%BAc%20~%20Download%20Lossless%2C%20500kbps%2C%20320kbps%20~%20Anh%20Nang%20Cua%20Anh%20-%20Duc%20Phuc.mp3" autoplay loop></audio>


<footer id="copyrightFooter">
  © 2025 Bản quyền thuộc về 
    Nguyễn Minh Chiến
  </a>
</footer>

<script>
  const galaxy = document.getElementById('galaxy');
  const loadingScreen = document.getElementById('loadingScreen');
  const errorScreen = document.getElementById('errorScreen');
  const audio = document.getElementById('galaxyAudio');

  let rotationX = 0;
  let rotationY = 0;
  let scale = 1;
  let isDragging = false;
  let lastMouseX = 0;
  let lastMouseY = 0;
  let initialDistance = 0;
  let initialScale = 1;
  const activeParticles = new Set();

  const isMobile = window.innerWidth <= 768;
  const isSmallMobile = window.innerWidth <= 480;
  const maxParticles = isSmallMobile ? 150 : isMobile ? 200 : 300;
  const particleInterval = isMobile ? 100 : 120;
  const starCount = isSmallMobile ? 250 : isMobile ? 350 : 500;

  let particleSpeedMultiplier = 1.3;

  // Demo galaxy data:
  const galaxyData = {
    colors: {
      love: '#ff4466',
      birthday: '#ff4466',
      date: '#ff4466',
      special: '#ff4466',
      heart: '#ff4466',
    },
    messages: [
      'Hoàng Ngọc Mai ❤️ ','Hoàng Ngọc Mai ❤️ ','Hoàng Ngọc Mai ❤️ ', 'I Love You ❤️', 'I Love You ❤️', 'I Love You ❤️', 'I Love You ❤️', '15/10/1999', 'Cảm Ơn vì đã bên Anh','15/10/1999','15/10/1999', 'Nguyễn Minh Chiến','07/11/1996','Nguyễn Minh Chiến','Yêu Bà Xã', 'Anh Yêu Em','Yêu Bà Xã','Anh Yêu Em','Anh Yêu Em', 'Yêu Bà Xã', '❤️', '💖'
      
    ],
    icons: [],
    images: [
      'https://i.ibb.co/GQgjMGYN/unnamed-1.jpg',
      'https://i.ibb.co/DHQPqZJD/unnamed-2.jpg',
      'https://i.ibb.co/zTK7MYQL/unnamed.jpg'
    ],
    song: 'https://dn720304.ca.archive.org/0/items/anh-nang-cua-anh-duc-phuc-download-lossless-500kbps-320kbps-anh-nang-cua-anh-duc-phuc/%C3%81nh%20N%E1%BA%AFng%20C%E1%BB%A7a%20Anh%20-%20%C4%90%E1%BB%A9c%20Ph%C3%BAc%20~%20Download%20Lossless%2C%20500kbps%2C%20320kbps%20~%20Anh%20Nang%20Cua%20Anh%20-%20Duc%20Phuc.mp3'
  };

  // Setup colors in CSS
  const style = document.createElement('style');
  style.textContent = `
    .text-particle.love { color: ${galaxyData.colors.love}; text-shadow: 0 0 15px ${galaxyData.colors.love}; }
    .text-particle.birthday { color: ${galaxyData.colors.birthday}; text-shadow: 0 0 15px ${galaxyData.colors.birthday}; }
    .text-particle.date { color: ${galaxyData.colors.date}; text-shadow: 0 0 20px ${galaxyData.colors.date}; }
    .text-particle.special { color: ${galaxyData.colors.special}; text-shadow: 0 0 15px ${galaxyData.colors.special}; }
    .text-particle.heart { color: ${galaxyData.colors.heart}; text-shadow: 0 0 20px ${galaxyData.colors.heart}; }
  `;
  document.head.appendChild(style);

  // Play background music
  if (galaxyData.song) {
    audio.src = galaxyData.song;
    audio.volume = 0.15;
    audio.play().catch(() => {});
    const playAudioOnUserGesture = () => {
      audio.play().catch(() => {});
      document.removeEventListener('touchstart', playAudioOnUserGesture);
      document.removeEventListener('click', playAudioOnUserGesture);
    };
    document.addEventListener('touchstart', playAudioOnUserGesture);
    document.addEventListener('click', playAudioOnUserGesture);
  }

  // Helper functions
  function getRandomMessage() {
    return galaxyData.messages[Math.floor(Math.random() * galaxyData.messages.length)];
  }

  function getRandomIcon() {
    return galaxyData.icons[Math.floor(Math.random() * galaxyData.icons.length)];
  }

  function getMessageClass(message) {
    const lower = message.toLowerCase();
    if (lower.includes('love') || lower.includes('yêu') || lower.includes('tình')) return 'love';
    if (lower.includes('birthday') || lower.includes('sinh nhật') || lower.includes('chúc mừng')) return 'birthday';
    if (/\d/.test(message) || message.includes('/')) return 'date';
    return 'special';
  }

  // Create stars
  function createStars() {
    for (let i = 0; i < starCount; i++) {
      const star = document.createElement('div');
      star.className = 'star';
      const angle = Math.random() * Math.PI * 10;
      const radius = Math.random() * (isMobile ? 800 : 1500) + (isMobile ? 200 : 300);
      const spiralHeight = Math.sin(angle) * (isMobile ? 100 : 150);
      const x = Math.cos(angle) * radius;
      const y = spiralHeight + (Math.random() - 0.5) * (isMobile ? 1000 : 2000);
      const z = Math.sin(angle) * radius * 0.5;
      star.style.left = `calc(50% + ${x}px)`;
      star.style.top = `calc(50% + ${y}px)`;
      star.style.transform = `translateZ(${z}px)`;
      star.style.opacity = Math.max(0.1, 1 - Math.abs(z) / (isMobile ? 800 : 1200));
      galaxy.appendChild(star);
    }
  }

  // Create text or icon particle
  function createTextParticle() {
    if (activeParticles.size >= maxParticles) return;

    const isIcon = Math.random() > 0.7;
    const element = document.createElement('div');

    if (isIcon) {
      element.className = 'text-particle heart';
      element.textContent = getRandomIcon();
      element.style.fontSize = (isMobile ? 15 : 25) + 'px';
    } else {
      const msg = getRandomMessage();
      element.className = 'text-particle ' + getMessageClass(msg);
      element.textContent = msg;
      element.style.fontSize = (isMobile ? 12 : 18) + 'px';
    }

    element.style.left = (Math.random() * 100) + '%';

    galaxy.appendChild(element);
    activeParticles.add(element);

    let startTime = null;
    const startY = -100;
    const endY = window.innerHeight + 100;
    const duration = (Math.random() * 5 + 4) * particleSpeedMultiplier;

    function animate(timestamp) {
      if (!startTime) startTime = timestamp;
      const progress = (timestamp - startTime) / (duration * 1000);

      if (progress < 1) {
        const currentY = startY + (endY - startY) * progress;
        const opacity = progress < 0.05 ? progress * 20 :
                        progress > 0.95 ? (1 - progress) * 20 :
                        1;
        element.style.transform = `translate3d(0, ${currentY}px, 0)`;
        element.style.opacity = opacity;
        requestAnimationFrame(animate);
      } else {
        if (element.parentNode) {
          element.parentNode.removeChild(element);
          activeParticles.delete(element);
        }
      }
    }

    requestAnimationFrame(animate);
  }

  // Create image particle
  function createImageParticle() {
    if (!galaxyData.images || galaxyData.images.length === 0 || activeParticles.size >= maxParticles) return;

    const img = document.createElement('img');
    img.src = galaxyData.images[Math.floor(Math.random() * galaxyData.images.length)];
    img.className = 'text-particle image-particle';

    img.style.width = (Math.random() * 60 + 80) + 'px'; // 60-100 px width
    img.style.height = 'auto';

    img.style.left = (Math.random() * 100) + '%';

    galaxy.appendChild(img);
    activeParticles.add(img);

    let startTime = null;
    const startY = -150;
    const endY = window.innerHeight + 150;
    const duration = (Math.random() * 3 + 3) * particleSpeedMultiplier;

    function animate(timestamp) {
      if (!startTime) startTime = timestamp;
      const progress = (timestamp - startTime) / (duration * 1000);

      if (progress < 1) {
        const currentY = startY + (endY - startY) * progress;
        const opacity = progress < 0.05 ? progress * 20 :
                        progress > 0.95 ? (1 - progress) * 20 :
                        1;
        img.style.transform = `translate3d(0, ${currentY}px, 0)`;
        img.style.opacity = opacity;
        requestAnimationFrame(animate);
      } else {
        if (img.parentNode) {
          img.parentNode.removeChild(img);
          activeParticles.delete(img);
        }
      }
    }

    requestAnimationFrame(animate);
  }

  // Update galaxy transform (rotate + scale)
  function updateGalaxyTransform() {
    galaxy.style.transform = `translate(-50%, -50%) scale(${scale}) rotateX(${rotationX}deg) rotateY(${rotationY}deg)`;
  }

  // Mouse and touch handlers
  galaxy.style.pointerEvents = 'auto';
  galaxy.style.touchAction = 'none';

  document.addEventListener('mousedown', (e) => {
    isDragging = true;
    lastMouseX = e.clientX;
    lastMouseY = e.clientY;
  });

  document.addEventListener('mouseup', () => {
    isDragging = false;
  });

  document.addEventListener('mousemove', (e) => {
    if (isDragging) {
      const deltaX = e.clientX - lastMouseX;
      const deltaY = e.clientY - lastMouseY;
      const sensitivity = 0.3;

      rotationY += deltaX * sensitivity;
      rotationX -= deltaY * sensitivity;

      rotationX = Math.min(90, Math.max(-90, rotationX));

      updateGalaxyTransform();

      lastMouseX = e.clientX;
      lastMouseY = e.clientY;
    }
  });

  document.addEventListener('wheel', (e) => {
    e.preventDefault();
    scale += -e.deltaY * 0.0015;
    scale = Math.min(2, Math.max(0.5, scale));
    updateGalaxyTransform();
  }, { passive: false });

  document.addEventListener('touchstart', (e) => {
    e.preventDefault();
    if (e.touches.length === 1) {
      isDragging = true;
      lastMouseX = e.touches[0].clientX;
      lastMouseY = e.touches[0].clientY;
    } else if (e.touches.length === 2) {
      isDragging = false;
      const dx = e.touches[1].clientX - e.touches[0].clientX;
      const dy = e.touches[1].clientY - e.touches[0].clientY;
      initialDistance = Math.sqrt(dx * dx + dy * dy);
      initialScale = scale;
    }
  }, { passive: false });

  document.addEventListener('touchmove', (e) => {
    e.preventDefault();
    if (e.touches.length === 1 && isDragging) {
      const deltaX = e.touches[0].clientX - lastMouseX;
      const deltaY = e.touches[0].clientY - lastMouseY;
      const sensitivity = 0.3;

      rotationY += deltaX * sensitivity;
      rotationX -= deltaY * sensitivity;
      rotationX = Math.min(90, Math.max(-90, rotationX));

      updateGalaxyTransform();

      lastMouseX = e.touches[0].clientX;
      lastMouseY = e.touches[0].clientY;
    } else if (e.touches.length === 2) {
      const touch1 = e.touches[0];
      const touch2 = e.touches[1];
      const currentDistance = Math.sqrt(
        Math.pow(touch2.clientX - touch1.clientX, 2) +
        Math.pow(touch2.clientY - touch1.clientY, 2)
      );

      const scaleChange = currentDistance / initialDistance;
      scale = Math.min(2, Math.max(0.5, initialScale * scaleChange));
      updateGalaxyTransform();
    }
  }, { passive: false });

  document.addEventListener('touchend', (e) => {
    if (e.touches.length === 0) {
      isDragging = false;
    } else if (e.touches.length === 1) {
      isDragging = true;
      lastMouseX = e.touches[0].clientX;
      lastMouseY = e.touches[0].clientY;
    }
  }, { passive: false });

  window.addEventListener('orientationchange', () => {
    setTimeout(() => {
      location.reload();
    }, 200);
  });

  // Load galaxy data & initialize
  function loadGalaxyData() {
    try {
      loadingScreen.style.display = 'none';

      createStars();

      setInterval(() => {
        // Randomly create particles, preferring text & icons, sometimes images
        const rand = Math.random();
        if (rand < 0.75) createTextParticle();
        else createImageParticle();

        // Clean old particles if too many (handled inside create functions)
      }, particleInterval);

      updateGalaxyTransform();
    } catch (err) {
      loadingScreen.style.display = 'none';
      errorScreen.style.display = 'flex';
      console.error('Lỗi tải dữ liệu galaxy:', err);
    }
  }

  loadGalaxyData();

  // Hide footer after 4 seconds
  setTimeout(() => {
    const footer = document.getElementById('copyrightFooter');
    if (footer) footer.style.display = 'none';
  }, 4000);

</script>

</body>
</html>
