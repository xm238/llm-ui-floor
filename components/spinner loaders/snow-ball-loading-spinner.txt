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
snow-ball-loading-spinner.tsx
export default function LoadingSpinner() {
  return (
    <div className="pl">
      <div className="pl__outer-ring"></div>
      <div className="pl__inner-ring"></div>
      <div className="pl__track-cover"></div>
      <div className="pl__ball">
        <div className="pl__ball-texture"></div>
        <div className="pl__ball-outer-shadow"></div>
        <div className="pl__ball-inner-shadow"></div>
        <div className="pl__ball-side-shadows"></div>
      </div>
    </div>
  );
}

demo.tsx
import LoadingSpinner from '../components/ui/snow-ball-loading-spinner';

export default function Default() {
  return <LoadingSpinner />;
}
```

Extend existing Tailwind 4 index.css with this code (or if project uses Tailwind 3, extend tailwind.config.js or globals.css):
```css
@import "tailwindcss";
@import "tw-animate-css";


@keyframes ball {
  from {
    transform: rotate(0) translateY(-6.5em);
  }
  50% {
    transform: rotate(180deg) translateY(-6em);
  }
  to {
    transform: rotate(360deg) translateY(-6.5em);
  }
}

@keyframes ballInnerShadow {
  from {
    transform: rotate(0);
  }
  to {
    transform: rotate(-360deg);
  }
}

@keyframes ballOuterShadow {
  from {
    transform: rotate(20deg);
  }
  to {
    transform: rotate(-340deg);
  }
}

@keyframes ballTexture {
  from {
    transform: translateX(0);
  }
  to {
    transform: translateX(50%);
  }
}

@keyframes trackCover {
  from {
    transform: rotate(0);
  }
  to {
    transform: rotate(360deg);
  }
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
