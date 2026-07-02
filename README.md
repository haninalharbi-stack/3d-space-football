# javaFX-3d-space-football
An interactive 3D arcade space game built from scratch using JavaFX, featuring physics simulation and cinematic camera movements.

# 3D Space Football Game 🌌⚽
**An interactive 3D arcade game built from scratch using JavaFX, showcasing advanced 3D graphics concepts, physics simulation, and custom camera animations.**

---

## 📌 Project Overview
This project is a space-themed 3D ball game developed entirely using JavaFX. The core gameplay challenges players to control a dynamically falling ball within a zero-gravity space environment, avoiding planetary obstacles and guiding its path using a movable ground platform. 

The primary objective is to keep the ball from dropping past the safety threshold by strategically positioning obstacles, all while enjoying a cinematic camera perspective.

---

## 🚀 Key Technical Features
- **Comprehensive 3D Scene Build:** Renders fully-textured 3D spheres representing Earth, Mars, Mercury, and Jupiter within an immersive space skybox.
- **Realistic Ball Physics:** Implements gravity-based acceleration, maximum speed caps, boundary wrapping behavior, and directional-dependent bounce calculations.
- **Dynamic Real-time Lighting:** Features ambient and directional point lights that provide instant visual feedback (e.g., shifting to green-yellow on collisions, and red during a Game Over).
- **Cinematic Camera Movement:** Implements custom cubic **Bézier curves** to manage smooth, error-free automated camera transitions and cinematic viewing angles.
- **Interactive Scoring System:** Tracks continuous gameplay progress and displays interactive overlays for score updates and system states.

---

## 🛠️ Tech Stack & Graphics Concepts
- **Language & Core Framework:** Java, JavaFX (3D Graphics Module).
- **Graphics Pipeline:** Custom Texture Mapping (Diffuse & Bump Mapping), 3D Transformations (Translation, Rotation, Scaling).
- **Animation Control:** Optimized `AnimationTimer` loop minimizing complex per-frame rendering operations.

---

## 🎮 Game Controls & User Guide
| Key Command | Action / Response |
| :---: | :--- |
| **`SPACEBAR`** | Start or Pause the ball animation |
| **`LEFT / RIGHT` Arrows** | Move the primary obstacle horizontally |
| **`UP / DOWN` Arrows** | Smoothly Zoom the camera view in and out |
| **`C` Key** | Enable/Disable automated Bézier curve cinematic camera tracking |
| **`R` Key** | Full game restart and position reset |

---

## 📸 User Interface & Visuals
Here is a preview of the interactive 3D gameplay and system states inside the JavaFX application:

### **Initial 3D Game Environment**
*Features the custom space-themed moon-texture ground, score system, and planetary objects:*
<img width="1246" height="635" alt="CG_project-haninAlharbi-F12 pdf and 1 more page - Profile 1 - Microsoft​ Edge 7_2_2026 11_00_32 PM" src="https://github.com/user-attachments/assets/24d5e759-275b-4ee8-b158-c285db9ae7b6" />

### **Active Collision State**
*Demonstrates dynamic collision feedback causing the ambient lighting to shift to a green-yellow hue:*
<img width="1313" height="683" alt="CG_project-haninAlharbi-F12 pdf and 1 more page - Profile 1 - Microsoft​ Edge 7_2_2026 11_00_43 PM" src="https://github.com/user-attachments/assets/a7c602a4-8241-48ce-b109-e7d736dd3e94" />

### **Game Over State**
*Showcases the ambient light shifting to red and the Game Over feedback when the ball drops past the threshold:*
<img width="1288" height="608" alt="CG_project-haninAlharbi-F12 pdf and 1 more page - Profile 1 - Microsoft​ Edge 7_2_2026 11_02_39 PM" src="https://github.com/user-attachments/assets/df1b9776-174c-48ac-a26c-3282b99d5275" />

---
## 📊 Core Graphics & Animation Implementation (Preview)
Here is a conceptual snippet showing how the game continuous updates are managed using JavaFX `AnimationTimer` and how cinematic camera interpolation is computed:

```java
// 1. Driving the Core Game Loop using AnimationTimer
AnimationTimer gameLoop = new AnimationTimer() {
    @Override
    public void handle(long now) {
        updateBallPhysics();    // Handles gravity, acceleration, and speed caps
        checkCollisions();       // Resolves directional bounce and ambient lighting
        updateCameraPath();     // Updates cinematic view matrices
    }
};
gameLoop.start();

// 2. Custom Cubic Bézier Curve Interpolation for Smooth Camera Transitions
public Point3D calculateBezierPoint(Point3D p0, Point3D p1, Point3D p2, Point3D p3, double t) {
    double u = 1 - t;
    double tt = t * t;
    double uu = u * u;
    double uuu = uu * u;
    double ttt = tt * t;

    // Cubic Bézier formula: P(t) = (1-t)³P₀ + 3(1-t)²tP₁ + 3(1-t)t²P₂ + t³P₃
    double x = uuu * p0.getX() + 3 * uu * t * p1.getX() + 3 * u * tt * p2.getX() + ttt * p3.getX();
    double y = uuu * p0.getY() + 3 * uu * t * p1.getY() + 3 * u * tt * p2.getY() + ttt * p3.getY();
    double z = uuu * p0.getZ() + 3 * uu * t * p1.getZ() + 3 * u * tt * p2.getZ() + ttt * p3.getZ();

    return new Point3D(x, y, z);
}
```

---
## 💡 Engineering Challenges & Solutions
### 1. 3D Camera Projection & Setup
- **Challenge:** Initializing the environment with correct perspective projection; the view frequently rendered empty spaces or clipped elements inappropriately.
- **Solution:** Configured an explicit `PerspectiveCamera` with exact near and far clipping planes relative to the object's spatial coordinates.

### 2. Directional Collision Responses
- **Challenge:** Standard simple bounds intersection caused unnatural bouncing trajectories that broke the space immersion.
- **Solution:** Engineered rigorous directional boundary testing to calculate the precise angle of incidence, enforcing smooth velocity decreases upon platform rebounds.
