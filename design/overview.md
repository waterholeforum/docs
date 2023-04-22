# Design System

Leverage Waterhole's design tokens, utilities, and components in your customizations.

The Waterhole Design System provides a foundation on which to build consistent user interfaces. It consists of three parts:

-   **Design tokens**, exposed as CSS variables, enabling a consistent orchestration of spacing, colors, and typography.

-   A limited collection of **utility classes** to streamline construction of common layouts and consumption of design tokens.

-   A range of **components**, exposed as CSS classes and sometimes Blade Components, abstracting frequently used visual styles and patterns.

The Design System is not exhaustive. Utility classes are only provided where they make sense as a way of encapsulating common declarations, but not to replace CSS in its entirety. You are encouraged to still use semantic class names to keep your code readable, apply additional styles beyond what the utility classes facilitate, and as an interface for other users to customize.

Similarly, Blade Components are only provided to encapsulate more complex HTML structures and behavior. For basic UI elements like buttons and inputs, use plain HTML elements and apply the CSS classes directly.
