# MWAD_EX05_image-carousel-in-react
## Date:22/10/2025

## AIM
To create a Image Carousel using React 

## ALGORITHM
### STEP 1 Initial Setup:
Input: A list of images to display in the carousel.

Output: A component displaying the images with navigation controls (e.g., next/previous buttons).

### Step 2 State Management:
Use a state variable (currentIndex) to track the index of the current image displayed.

The carousel starts with the first image, so initialize currentIndex to 0.

### Step 3 Navigation Controls:
Next Image: When the "Next" button is clicked, increment currentIndex.

If currentIndex is at the end of the image list (last image), loop back to the first image using modulo:
currentIndex = (currentIndex + 1) % images.length;

Previous Image: When the "Previous" button is clicked, decrement currentIndex.

If currentIndex is at the beginning (first image), loop back to the last image:
currentIndex = (currentIndex - 1 + images.length) % images.length;

### Step 4 Displaying the Image:
The currentIndex determines which image is displayed.

Using the currentIndex, display the corresponding image from the images list.

### Step 5 Auto-Rotation:
Set an interval to automatically change the image after a set amount of time (e.g., 3 seconds).

Use setInterval to call the nextImage() function at regular intervals.

Clean up the interval when the component unmounts using clearInterval to prevent memory leaks.

## PROGRAM
## App.jsx
```
import React from 'react'
import ImageCarousel from './ImageCarousel'

export default function App() {
  const artworks = [
    {
      src: '/public/art1.jpg',
      alt: 'Digital Landscape',
      caption: '“Serenity in Pixels” — Digital Landscape (2024)',
    },
    {
      src: '/public/art2.jpg',
      alt: 'Sketch Portrait',
      caption: '“Lines of Thought” — Hand-drawn Portrait',
    },
    {
      src: '/public/art3.png',
      alt: 'Abstract Painting',
      caption: '“Chaos & Calm” — Acrylic on Canvas',
    },
    {
      src: '/public/art4.jpg',
      alt: 'Concept Illustration',
      caption: '“Beyond Reality” — Sci-Fi Concept Art',
    },
  ]

  return (
    <div style={{ textAlign: 'center', fontFamily: 'Poppins, sans-serif' }}>
      <h1 style={{ marginTop: '30px', color: '#2d0202ff' }}>🎨 My Art Portfolio</h1>
      <p style={{ color: '#0a202fff' }}>A showcase of my digital artworks and illustrations</p>
      <ImageCarousel images={artworks} autoPlayInterval={4000} />
    </div>
  )
}


```
## App.css
```
#root {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;
  text-align: center;
}

.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.react:hover {
  filter: drop-shadow(0 0 2em #61dafbaa);
}

@keyframes logo-spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

@media (prefers-reduced-motion: no-preference) {
  a:nth-of-type(2) .logo {
    animation: logo-spin infinite 20s linear;
  }
}

.card {
  padding: 2em;
}

.read-the-docs {
  color: #888;
}

```
## ImageCarousel.css
```
.carousel-container {
  position: relative;
  max-width: 850px;
  margin: 50px auto;
  overflow: hidden;
  border-radius: 15px;
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.25);
  background: linear-gradient(145deg, #1a1a1a, #2e2e2e);
}

.carousel-inner {
  position: relative;
  height: 500px;
}

.carousel-item {
  position: absolute;
  width: 100%;
  height: 100%;
  opacity: 0;
  transform: scale(1.02);
  transition: opacity 0.8s ease, transform 1s ease;
}

.carousel-item.active {
  opacity: 1;
  transform: scale(1);
  z-index: 1;
}

.carousel-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 15px;
}

.carousel-caption {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 15px;
  text-align: center;
  background: rgba(0, 0, 0, 0.55);
  color: #f5f5f5;
  font-size: 18px;
  font-weight: 400;
}

/* Controls */
.carousel-controls {
  position: absolute;
  top: 50%;
  width: 100%;
  display: flex;
  justify-content: space-between;
  padding: 0 15px;
  transform: translateY(-50%);
}

.control-btn {
  background: rgba(255, 255, 255, 0.2);
  color: white;
  border: none;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  cursor: pointer;
  font-size: 18px;
  transition: all 0.3s ease;
}

.control-btn:hover {
  background: rgba(255, 255, 255, 0.4);
}

/* Indicators */
.carousel-indicators {
  position: absolute;
  bottom: 15px;
  display: flex;
  justify-content: center;
  width: 100%;
  gap: 8px;
}

.indicator {
  width: 12px;
  height: 12px;
  background: rgba(255, 255, 255, 0.5);
  border-radius: 50%;
  cursor: pointer;
}

.indicator.active {
  background: #fff;
}
```
## ImageCarousel.jsx
```
import { useState, useEffect } from 'react'
import './ImageCarousel.css'

const ImageCarousel = ({ images, autoPlayInterval = 4000 }) => {
  const [currentIndex, setCurrentIndex] = useState(0)
  const [isPlaying, setIsPlaying] = useState(true)

  useEffect(() => {
    let interval
    if (isPlaying) {
      interval = setInterval(() => {
        setCurrentIndex((prevIndex) => (prevIndex + 1) % images.length)
      }, autoPlayInterval)
    }
    return () => clearInterval(interval)
  }, [isPlaying, images.length, autoPlayInterval])

  const goToPrevious = () => {
    setCurrentIndex((prevIndex) => (prevIndex - 1 + images.length) % images.length)
  }

  const goToNext = () => {
    setCurrentIndex((prevIndex) => (prevIndex + 1) % images.length)
  }

  return (
    <div className="carousel-container">
      <div className="carousel-inner">
        {images.map((image, index) => (
          <div
            key={index}
            className={`carousel-item ${index === currentIndex ? 'active' : ''}`}
          >
            <img src={image.src} alt={image.alt || `Artwork ${index + 1}`} />
            {image.caption && <div className="carousel-caption">{image.caption}</div>}
          </div>
        ))}
      </div>

      <div className="carousel-controls">
        <button className="control-btn" onClick={goToPrevious}>
          ⬅
        </button>
        <button className="control-btn" onClick={goToNext}>
          ➡
        </button>
        <button className="control-btn" onClick={() => setIsPlaying(!isPlaying)}>
          {isPlaying ? '❚❚' : '▶'}
        </button>
      </div>

      <div className="carousel-indicators">
        {images.map((_, index) => (
          <span
            key={index}
            className={`indicator ${index === currentIndex ? 'active' : ''}`}
            onClick={() => setCurrentIndex(index)}
          />
        ))}
      </div>
    </div>
  )
}

export default ImageCarousel
```



## OUTPUT

<img width="1914" height="955" alt="Screenshot 2025-10-22 112941" src="https://github.com/user-attachments/assets/1f3ec630-bc0d-482b-8456-81560d2e7f14" />

## RESULT
The program for creating Image Carousel using React is executed successfully.
