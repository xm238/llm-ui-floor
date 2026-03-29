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
expanding-cards.tsx
"use client";

import * as React from "react";
import {
  Pyramid,
  Castle,
  Mountain,
  TowerControl,
  Building,
  Landmark,
} from "lucide-react";
import { cn } from "@/lib/utils"; 

export interface CardItem {
  id: string | number;
  title: string;
  description: string;
  imgSrc: string;
  icon: React.ReactNode;
  linkHref: string;
}

interface ExpandingCardsProps extends React.HTMLAttributes<HTMLUListElement> {
  items: CardItem[];
  defaultActiveIndex?: number;
}

export const ExpandingCards = React.forwardRef<
  HTMLUListElement,
  ExpandingCardsProps
>(({ className, items, defaultActiveIndex = 0, ...props }, ref) => {
  const [activeIndex, setActiveIndex] = React.useState<number | null>(
    defaultActiveIndex,
  );
  
  const [isDesktop, setIsDesktop] = React.useState(false);

  React.useEffect(() => {
    const handleResize = () => {
      setIsDesktop(window.innerWidth >= 768);
    };
    handleResize();
    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);

  const gridStyle = React.useMemo(() => {
    if (activeIndex === null) return {};
    
    if (isDesktop) {
      const columns = items
        .map((_, index) => (index === activeIndex ? "5fr" : "1fr"))
        .join(" ");
      return { gridTemplateColumns: columns };
    } else {
      const rows = items
        .map((_, index) => (index === activeIndex ? "5fr" : "1fr"))
        .join(" ");
      return { gridTemplateRows: rows };
    }
  }, [activeIndex, items.length, isDesktop]);

  const handleInteraction = (index: number) => {
    setActiveIndex(index);
  };

  return (
    <ul
      className={cn(
        "w-full max-w-6xl gap-2",
        "grid",
        "h-[600px] md:h-[500px]",
        "transition-[grid-template-columns,grid-template-rows] duration-500 ease-out",
        className,
      )}
      style={{
        ...gridStyle,
        ...(isDesktop 
          ? { gridTemplateRows: '1fr' }
          : { gridTemplateColumns: '1fr' }
        )
      }}
      ref={ref}
      {...props}
    >
      {items.map((item, index) => (
        <li
          key={item.id}
          className={cn(
            "group relative cursor-pointer overflow-hidden rounded-lg border bg-card text-card-foreground shadow-sm",
            "md:min-w-[80px]",
            "min-h-0 min-w-0"
          )}
          onMouseEnter={() => handleInteraction(index)}
          onFocus={() => handleInteraction(index)}
          onClick={() => handleInteraction(index)}
          tabIndex={0}
          data-active={activeIndex === index}
        >
          <img
            src={item.imgSrc}
            alt={item.title}
            className="absolute inset-0 h-full w-full object-cover transition-all duration-300 ease-out group-data-[active=true]:scale-100 group-data-[active=true]:grayscale-0 scale-110 grayscale"
          />
          <div className="absolute inset-0 bg-gradient-to-t from-black/80 via-black/40 to-transparent" />

          <article
            className="absolute inset-0 flex flex-col justify-end gap-2 p-4"
          >
            <h3 className="hidden origin-left rotate-90 text-sm font-light uppercase tracking-wider text-white/80 opacity-100 transition-all duration-300 ease-out md:block group-data-[active=true]:opacity-0">
              {item.title}
            </h3>

            <div className="text-white/90 opacity-0 transition-all duration-300 delay-75 ease-out group-data-[active=true]:opacity-100">
              {item.icon}
            </div>

            <h3 className="text-xl font-bold text-white opacity-0 transition-all duration-300 delay-150 ease-out group-data-[active=true]:opacity-100">
              {item.title}
            </h3>

            <p className="w-full max-w-xs text-sm text-white/80 opacity-0 transition-all duration-300 delay-225 ease-out group-data-[active=true]:opacity-100">
              {item.description}
            </p>
          </article>
        </li>
      ))}
    </ul>
  );
});
ExpandingCards.displayName = "ExpandingCards";

demo.tsx
// demo.tsx
import {
  Pyramid,
  Castle,
  Mountain,
  TowerControl,
  Building,
  Landmark,
} from "lucide-react";
import { ExpandingCards, CardItem } from "@/components/ui/expanding-cards";

const architecturalWonders: CardItem[] = [
  {
    id: "pyramids-giza",
    title: "Pyramids of Giza",
    description:
      "The last surviving of the Seven Wonders of the Ancient World, these monumental tombs have stood for over 4,500 years.",
    imgSrc:
      "https://images.unsplash.com/photo-1568322445389-f64ac2515020?auto=format&fit=crop&w=1200",
    icon: <Pyramid size={24} />,
    linkHref: "#",
  },
  {
    id: "great-wall",
    title: "Great Wall of China",
    description:
      "A vast series of fortifications stretching thousands of miles, built to protect Chinese states and empires against raids.",
    imgSrc:
      "https://images.unsplash.com/flagged/photo-1552553030-837c9c2b0fac?w=900&auto=format&fit=crop&q=60",
    icon: <Castle size={24} />,
    linkHref: "#",
  },
  {
    id: "machu-picchu",
    title: "Machu Picchu",
    description:
      "An Incan citadel set high in the Andes Mountains in Peru, renowned for its sophisticated dry-stone walls that fuse huge blocks.",
    imgSrc:
      "https://images.unsplash.com/photo-1585329017236-46abbe0f301f?w=900&auto=format&fit=crop&q=60&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxzZWFyY2h8M3x8bWFjaHUlMjBwaWNodXxlbnwwfDF8MHx8fDA%3D",
    icon: <Mountain size={24} />,
    linkHref: "#",
  },
  {
    id: "eiffel-tower",
    title: "Eiffel Tower",
    description:
      "A global cultural icon of France and one of the most recognizable structures in the world, offering panoramic views of Paris.",
    imgSrc:
      "https://images.unsplash.com/photo-1511739001486-6bfe10ce785f?auto=format&fit=crop&w=1200",
    icon: <TowerControl size={24} />,
    linkHref: "#",
  },
  {
    id: "burj-khalifa",
    title: "Burj Khalifa",
    description:
      "The world's tallest building, a modern architectural marvel in Dubai that pierces the sky at over 828 meters.",
    imgSrc:
      "https://images.unsplash.com/photo-1572364769167-198dcb7b520c?w=900&auto=format&fit=crop&q=60&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxzZWFyY2h8Mnx8YnVyaiUyMGtoYWxpZmF8ZW58MHwxfDB8fHww",
    icon: <Building size={24} />,
    linkHref: "#",
  },
  {
    id: "taj-mahal",
    title: "Taj Mahal",
    description:
      "An immense mausoleum of white marble, built in Agra between 1631 and 1648 by order of the Mughal emperor Shah Jahan.",
    imgSrc:
      "https://plus.unsplash.com/premium_photo-1697730352959-a77dac39c883?w=900&auto=format&fit=crop&q=60&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxzZWFyY2h8NXx8dGFqJTIwbWFoYWx8ZW58MHwxfDB8fHww",
    icon: <Landmark size={24} />,
    linkHref: "#",
  },
  {
    id: "colosseum",
    title: "The Colosseum",
    description:
      "The largest ancient amphitheater ever built, it remains the largest standing amphitheater in the world today.",
    imgSrc:
      "https://images.unsplash.com/photo-1552832230-c0197dd311b5?auto=format&fit=crop&w=1200",
    icon: <Landmark size={24} />,
    linkHref: "#",
  },
];

export default function ExpandingCardsDemo() {
  return (
    <div className="flex w-full flex-col items-center justify-center space-y-8 bg-background p-4 md:p-8">
      <div className="text-center">
        <h1 className="text-3xl font-bold tracking-tight text-foreground sm:text-4xl">
          Architectural Wonders
        </h1>
        <p className="mt-4 max-w-2xl text-lg text-muted-foreground">
          Explore humanity's most ambitious and breathtaking creations. Hover or
          click on a card to unveil its story.
        </p>
      </div>
      <ExpandingCards items={architecturalWonders} defaultActiveIndex={0} />
    </div>
  );
}

```

Install NPM dependencies:
```bash
lucide-react
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
