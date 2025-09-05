# AGENTS.md

## Project Overview

This is a production-ready Vite + React boilerplate with TypeScript, built for scalability and developer experience. It's a batteries-included template with modern tooling, state management, routing, testing, and UI components.

**Key Technologies:**
- Vite + React 19 + TypeScript (strict mode)
- TanStack Router (typesafe routing)
- TanStack Query (server state)
- Zustand (client state)
- Tailwind CSS + HeadlessUI
- React Hook Form + Zod validation
- Vitest (unit tests) + Playwright (e2e tests)
- Storybook for component development
- i18next for internationalization

## Setup Commands

- **Install dependencies**: `pnpm install`
- **Start dev server**: `pnpm dev` (runs on default Vite port)
- **Run Storybook**: `pnpm storybook` (runs on port 6006)
- **Run both dev server and Storybook**: `pnpm all` (runs both concurrently)
- **Run type checking and linting**: `pnpm check` (runs both concurrently)
- **Build for production**: `pnpm build`
- **Preview production build**: `pnpm preview`

## Development Workflow

### Package Manager
- **Always use `pnpm`** - this project is configured for pnpm, not npm
- Lock file: `pnpm-lock.yaml`
- Install new packages: `pnpm add <package>` or `pnpm add -D <package>`

### Code Style & Linting
- **TypeScript strict mode** enabled
- **ESLint**: Strict configuration with TypeScript, React, accessibility, and Unicorn rules
- **Prettier**: Opinionated formatting (integrated with ESLint)
- **File naming**: Use PascalCase for components, camelCase for utilities
- **Import style**: Use consistent type imports (`import type`) when importing types

### Component Organization
Follow this structure for new components:
```
src/components/[category]/ComponentName/
├── ComponentName.tsx
├── ComponentName.stories.tsx  
├── ComponentName.test.tsx
└── index.ts
```

Categories:
- `ui/` - Reusable UI components
- `forms/` - Form-specific components  
- `layout/` - Layout and navigation components
- `charts/` - Data visualization components (uses Nivo)
- `utils/` - Utility components (development tools, etc.)

### Directory Structure
- `src/pages/` - Page components (used by TanStack Router)
- `src/routes/` - Route definitions and configuration
- `src/features/` - Feature-specific code (components, hooks, types)
- `src/hooks/` - Custom React hooks
- `src/store/` - Zustand stores
- `src/common/` - Shared utilities, types, and configuration
- `src/assets/locales/` - i18n translation files

## Testing Instructions

### Unit Testing (Vitest + React Testing Library)
- **Run all tests**: `pnpm test` (runs unit + e2e)
- **Run unit tests only**: `pnpm test:unit` (watch mode)
- **Run with coverage**: `pnpm test:unit:coverage`
- **Test files**: Colocated with components (`.test.tsx`)
- **Test utilities**: Use React Testing Library, avoid implementation details

### End-to-End Testing (Playwright)
- **Run e2e tests**: `pnpm test:e2e`
- **View test reports**: `pnpm test:e2e:report`
- **Test files**: Located in `e2e/` directory (`.spec.ts`)

### Before Committing
Always run code quality checks and testing:
```bash
pnpm check && pnpm test
```
Or run them individually:
```bash
pnpm lint && pnpm test
```

## Design System & Component Design Rules

### Component Architecture Principles
- **Single Responsibility**: Each component should have one clear purpose
- **Composition over Inheritance**: Build complex UIs by composing smaller components
- **Reusability**: Design components to be reusable across different contexts
- **Accessibility**: All components must follow WCAG guidelines and use semantic HTML
- **Performance**: Use React.memo() for expensive components, lazy loading for heavy components

### Component Categories & Guidelines

#### UI Components (`src/components/ui/`)
**Purpose**: Reusable, generic UI elements used across the application
**Examples**: Button, Input, Modal, Card, Badge, Avatar
**Rules**:
- Must be framework-agnostic (no business logic)
- Should accept common props like `className`, `children`, `disabled`
- Must include TypeScript interfaces for all props
- Should support theming via Tailwind classes
- Must be accessible (ARIA labels, keyboard navigation)

#### Form Components (`src/components/forms/`)
**Purpose**: Form-specific components with validation and state management
**Examples**: LoginForm, ContactForm, SearchInput, FormField
**Rules**:
- Use React Hook Form for state management
- Implement Zod schemas for validation
- Include error handling and display
- Support controlled and uncontrolled modes
- Must include proper labels and accessibility

#### Layout Components (`src/components/layout/`)
**Purpose**: Structural components that define page layout and navigation
**Examples**: Header, Footer, Sidebar, Navigation, Container
**Rules**:
- Should be responsive by default
- Use CSS Grid or Flexbox for layouts
- Support different viewport sizes
- Include proper semantic HTML structure
- Handle navigation state and routing

#### Chart Components (`src/components/charts/`)
**Purpose**: Data visualization components using Nivo library
**Categories**: `bar/`, `line/`, `pie/`
**Rules**:
- Use Nivo library for consistency
- Support responsive design
- Include proper data type definitions
- Provide customizable colors and themes
- Include loading and error states

### Feature-Based Architecture (`src/features/`)

#### Feature Structure
Each feature should be self-contained and follow this structure:
```
src/features/feature-name/
├── components/          # Feature-specific components
├── hooks/              # Feature-specific custom hooks
├── services/           # API calls and business logic
├── types/              # TypeScript interfaces and types
├── utilities/          # Feature-specific utility functions
└── index.ts           # Public API exports
```

#### Feature Design Rules
- **Encapsulation**: Keep feature code contained within its directory
- **Public API**: Export only what other features need via index.ts
- **Dependencies**: Features should not directly import from other features
- **Shared Code**: Use `src/common/` for code shared across features

### Component File Structure

#### Required Files for Each Component
```
ComponentName/
├── ComponentName.tsx        # Main component implementation
├── ComponentName.stories.tsx # Storybook stories
├── ComponentName.test.tsx   # Unit tests
├── ComponentName.types.ts   # TypeScript interfaces (if complex)
└── index.ts                # Export declarations
```

#### Component Implementation Rules
- **Props Interface**: Always define a clear props interface
- **Default Props**: Use default parameters instead of defaultProps
- **Forward Refs**: Use forwardRef for components that need DOM access
- **Error Boundaries**: Wrap components that might fail
- **Loading States**: Include loading, error, and empty states

### Styling Guidelines

#### Tailwind CSS Best Practices
- **Utility Classes**: Prefer utility classes over custom CSS
- **Responsive Design**: Use responsive prefixes (sm:, md:, lg:, xl:)
- **Dark Mode**: Support dark mode with `dark:` prefix
- **Component Variants**: Use conditional classes for different states
- **Custom Properties**: Use CSS custom properties for dynamic values

#### Design Tokens
- **Spacing**: Use rem units with Tailwind spacing scale
- **Colors**: Use semantic color names from Tailwind palette
- **Typography**: Use Tailwind typography classes
- **Shadows**: Use Tailwind shadow utilities
- **Borders**: Use consistent border radius and widths

### State Management Patterns

#### Component State (React useState)
- Use for local component state only
- Prefer useReducer for complex state logic
- Keep state as close to where it's used as possible

#### Global State (Zustand)
- Use for application-wide state
- Create separate stores for different domains
- Include TypeScript interfaces for store shape
- Use selectors to prevent unnecessary re-renders

#### Server State (TanStack Query)
- Use for all API data fetching
- Implement proper caching strategies
- Handle loading, error, and success states
- Use mutations for data updates

#### Form State (React Hook Form)
- Use for all form handling
- Implement Zod validation schemas
- Handle form submission and errors
- Support field-level validation

### Testing Strategy

#### Unit Tests (Required for all components)
- Test component rendering
- Test user interactions
- Test edge cases and error states
- Mock external dependencies
- Aim for 80%+ code coverage

#### Integration Tests
- Test component interactions
- Test data flow between components
- Test routing and navigation
- Use realistic data scenarios

#### Accessibility Testing
- Test keyboard navigation
- Test screen reader compatibility
- Validate ARIA attributes
- Check color contrast ratios

### Performance Guidelines

#### Component Optimization
- Use React.memo() for pure components
- Implement useMemo() for expensive calculations
- Use useCallback() for event handlers in dependency arrays
- Lazy load heavy components and routes

#### Bundle Optimization
- Use dynamic imports for code splitting
- Optimize images and assets
- Remove unused dependencies
- Minimize bundle size with tree shaking

### Documentation Requirements

#### Component Documentation
- Include JSDoc comments for all props
- Document component purpose and usage
- Provide examples in Storybook
- Document accessibility features
- Include migration guides for breaking changes

#### Storybook Stories
- Create stories for all component variants
- Include interactive controls (args)
- Document component states (loading, error, success)
- Provide usage examples and best practices

## Code Conventions

### TypeScript
- Use strict mode settings
- Explicit return types for functions: `function example(): ReturnType`
- Prefer `interface` over `type` for object shapes
- Use generic types: `Array<T>` instead of `T[]`
- Import types separately: `import type { Type } from './module'`

### React Patterns
- Use functional components with hooks
- Prefer composition over inheritance
- Use `React.FC` type for components when needed
- Custom hooks should start with `use`
- Event handlers should start with `handle`

### Forms & Validation
- Use React Hook Form for all forms
- Use Zod for schema validation
- Integrate with `@hookform/resolvers/zod`

### State Management
- **Client state**: Use Zustand for global state
- **Server state**: Use TanStack Query for API calls
- **Form state**: Use React Hook Form
- **URL state**: Use TanStack Router search params

### Styling
- **Primary**: Tailwind CSS utility classes
- **Components**: HeadlessUI for accessible components
- **Icons**: Heroicons (imported from `@heroicons/react`)
- **Dark mode**: Configured via Tailwind's dark mode classes

## Internationalization (i18n)

- **Library**: react-i18next
- **Configuration**: `src/common/i18n.ts`
- **Translation files**: `src/assets/locales/[lang]/translations.json`
- **Usage**: `const { t } = useTranslation()`
- **Supported locales**: English (`en`), Spanish (`es`)

## Development Tools

### DevTools (Development Only)
- TanStack Query DevTools (bottom left corner)
- TanStack Router DevTools (bottom right corner) 
- TanStack Table DevTools (inline with tables)
- React Hook Form DevTools (top right corner)
- Zustand DevTools (via Redux DevTools extension)

### Storybook
- **Start**: `pnpm storybook`
- **Build**: `pnpm storybook:build`
- **Story files**: `*.stories.tsx` (colocated with components)
- **Configuration**: `.storybook/main.ts` and `.storybook/preview.ts`

## Build & Deployment

### Development Build
- `pnpm dev` - starts Vite dev server with HMR

### Production Build  
- `pnpm build` - TypeScript compilation + Vite build
- Output: `dist/` directory
- Preview: `pnpm preview`

### Docker Deployment
1. `pnpm build`
2. `docker build . -t <container_name>`
3. `docker run -p <port>:80 <container_name>`

## Git Workflow

### Commit Messages
- **Format**: Conventional Commits specification
- **Tool**: Commitizen CLI (`git cz` or automatic via Husky)
- **Validation**: Commitlint ensures proper format
- **Hooks**: Husky manages pre-commit and commit-msg hooks

### Before Committing
- Husky runs automatic checks
- Format code: `pnpm format`
- Fix linting: `pnpm lint:fix`
- Run tests: `pnpm test`

## Important Notes

### Package-Specific Considerations
- **Faker.js**: Use localized imports (`@faker-js/faker/locale/en`) to avoid large bundle sizes
- **Vite v7**: Some Storybook addons may show peer dependency warnings (safe to ignore)
- **React 19**: Latest version with new features and performance improvements

### File Structure Rules
- README files in empty directories are placeholders - remove when adding real files
- `routeTree.gen.ts` is auto-generated by TanStack Router - don't edit manually
- Development tools are automatically excluded from production builds

### Performance Considerations
- Use `rem` units for spacing/sizing (pixel equivalents in comments)
- Lazy load routes and heavy components
- Optimize images and assets in `public/` directory
- Use React.memo() for expensive components when needed

## Security

- No sensitive data in environment files committed to git
- Use environment variables for API endpoints and keys
- Validate all user inputs with Zod schemas
- Follow React security best practices (avoid dangerouslySetInnerHTML)

## Troubleshooting

### Common Issues
- **npm errors**: Use `pnpm` instead of `npm`
- **TypeScript errors**: Check `tsconfig.json` and ensure proper imports
- **Storybook issues**: Ensure all dependencies are compatible versions
- **Build failures**: Run `pnpm lint` and fix all issues first
- **Test failures**: Check that all imports and mocks are properly configured
