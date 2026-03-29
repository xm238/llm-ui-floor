You are given a task to integrate an existing React component in the codebase

The codebase should support:
- shadcn project structure  
- Tailwind CSS
- Typescript

If it doesn't, provide instructions on how to setup project via shadcn CLI, install Tailwind or Typescript.

Determine the default path for components and styles. 
If default path for components is not /components/ui, provide instructions on why it's important to create this folder
Copy-paste this component to /components/ui folder:
```tsx
slideshow.tsx
import React, { useState } from "react";


const slides = [
  {
    img: "https://cdn.cosmos.so/8b0252bd-cb64-45f4-aef8-672c7f628f76?format=jpeg",
    text: ["BETWEEN SHADOW", "AND LIGHT"],
  },
  {
    img: "https://cdn.cosmos.so/7b3f4c48-ec63-4bac-b472-910c037a0eb4?format=jpeg",
    text: ["SILENCE SPEAKS", "THROUGH FORM"],
  },
  {
    img: "https://cdn.cosmos.so/444502b9-4cb9-4f14-a068-f0213df08729?format=jpeg",
    text: ["ESSENCE BEYOND", "PERCEPTION"],
  },
  {
    img: "https://cdn.cosmos.so/ef511e17-a35b-42e6-9122-2754bbd2ad7e?format=jpeg",
    text: ["TRUTH IN", "EMPTINESS"],
  },
  {
    img: "https://cdn.cosmos.so/cf68a397-080a-437a-994e-69dedd9e6e06?format=jpeg",
    text: ["SURRENDER TO", "THE VOID"],
  },
];

export default function Component() {
  const [current, setCurrent] = useState(0);

  const nextSlide = () => setCurrent((prev) => (prev + 1) % slides.length);
  const prevSlide = () =>
    setCurrent((prev) => (prev - 1 + slides.length) % slides.length);

  return (
    <div className="slideshow">
      {slides.map((slide, i) => (
        <div
          key={i}
          className={`slide ${i === current ? "active" : ""}`}
          style={{ backgroundImage: `url(${slide.img})` }}
        >
          <div className="slide-text">
            {slide.text.map((t, j) => (
              <span key={j}>{t}</span>
            ))}
          </div>
        </div>
      ))}

      {/* Controls */}
      <button className="nav left" onClick={prevSlide}>
        ←
      </button>
      <button className="nav right" onClick={nextSlide}>
        →
      </button>

      {/* Counter */}
      <div className="counter">
        0{current + 1} / 0{slides.length}
      </div>
    </div>
  );
}


demo.tsx
import  Component  from "@/components/ui/slideshow";

export default function DemoOne() {
  return <Component />;
}

```

Implementation Guidelines
 1. Analyze the component structure and identify all required dependencies
 2. Review the component's argumens and state
 3. Identify any required context providers or hooks and install them
 4. Questions to Ask
 - What data/props will be passed to this component?
 - Are there any specific state management requirements?
 - Are there any required assets (images, icons, etc.)?
 - What is the expected responsive behavior?
 - What is the best place to use this component in the app?

Steps to integrate
 0. Copy paste all the code above in the correct directories
 1. Install external dependencies
 2. Fill image assets with Unsplash stock images you know exist
 3. Use lucide-react icons for svgs or logos if component requires them
