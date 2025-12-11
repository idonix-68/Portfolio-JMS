# Portfolio-JMS
Portfolio dgn Agentic AI
Implementation Plan
⦁	[x] 1. Set up project structure and base HTML
⦁	[x] 1.1 Create index.html with HTML5 boilerplate and meta tags
⦁	Add DOCTYPE, html lang attribute, charset, viewport meta tags
⦁	Add page title
⦁	Requirements: 7.3
⦁	[x] 1.2 Add semantic HTML structure with empty sections
⦁	Create header, section (projects, about, contact), and footer elements
⦁	Add unique id attributes to each section
⦁	Requirements: 7.1, 7.2
⦁	[x] 1.3 Write property test for semantic HTML structure
⦁	Property 3: Semantic HTML Structure
⦁	Validates: Requirements 7.1
⦁	[x] 1.4 Write property test for section ID uniqueness
⦁	Property 4: Section ID Uniqueness
⦁	Validates: Requirements 7.2
⦁	[x] 2. Implement Hero Section
⦁	[x] 2.1 Create hero section HTML structure
⦁	Add greeting text, name heading (h1), role text
⦁	Add tagline paragraph with .tagline class
⦁	Requirements: 1.1, 1.2, 1.3
⦁	[x] 2.2 Add skill chips container
⦁	Create .chips container with span elements for HTML, CSS, JavaScript, React
⦁	Requirements: 1.4
⦁	[x] 2.3 Add CTA buttons
⦁	Create primary button "View Projects" linking to #projects
⦁	Create secondary button "Contact Me" linking to #contact
⦁	Requirements: 1.5, 1.6

⦁	[x] 3. Implement Projects Section
⦁	[x] 3.1 Create projects section with heading
⦁	Add section with id="projects" and h2 heading
⦁	Create grid container for project cards
⦁	Requirements: 2.1
⦁	[x] 3.2 Implement project card component structure
⦁	Create 3 article elements with .card class
⦁	Each card: h3 title, description p, tech stack small, links p
⦁	Requirements: 2.2, 2.3, 2.4, 2.5
⦁	[x] 3.3 Write property test for project card completeness
⦁	Property 1: Project Card Completeness
⦁	Validates: Requirements 2.2, 2.3, 2.4, 2.5
⦁	[x] 3.4 Write property test for project count bounds
⦁	Property 2: Project Count Bounds
⦁	Validates: Requirements 2.1
⦁	[x] 4. Implement About Section
⦁	[x] 4.1 Create about section with content
⦁	Add section with id="about" and h2 heading
⦁	Add bio paragraph describing developer background and skills
⦁	Requirements: 3.1, 3.2
⦁	[x] 5. Implement Contact Section and Footer
⦁	[x] 5.1 Create contact section with links
⦁	Add section with id="contact" and h2 heading
⦁	Add email link with mailto: protocol
⦁	Add GitHub and LinkedIn links
⦁	Requirements: 4.1, 4.2, 4.3, 4.4
⦁	[x] 5.2 Create footer with dynamic year
⦁	Add footer element with copyright text
⦁	Add span with id="year" for dynamic year
⦁	Add script to update year on page load
⦁	Requirements: 6.5
⦁	[x] 6. Implement CSS Styling
⦁	[x] 6.1 Add base styles and color scheme
⦁	Set body background (#0f172a), text color (#e5e7eb)
⦁	Set system-ui font family
⦁	Add link color (#38bdf8)
⦁	Requirements: 6.1, 6.2, 6.4
⦁	[x] 6.2 Style hero section
⦁	Style .hero container with flexbox layout
⦁	Style h1 heading size
⦁	Style .tagline with muted color
⦁	Requirements: 1.1, 1.2, 1.3
⦁	[x] 6.3 Style buttons and chips
⦁	Style .btn-primary with accent background and rounded corners
⦁	Style .btn-secondary with border and transparent background
⦁	Style .chips span elements with dark background
⦁	Requirements: 1.4, 1.5, 1.6, 6.2
⦁	[x] 6.4 Style project cards and grid
⦁	Style .card with dark background, border, and rounded corners
⦁	Style .grid with CSS Grid layout
⦁	Add responsive grid columns for desktop (3 columns)
⦁	Requirements: 2.1, 2.2
⦁	[x] 6.5 Add responsive styles
⦁	Add media query for mobile layout (single column)
⦁	Adjust hero layout for mobile
⦁	Requirements: 5.1, 5.3, 6.3
⦁	[x] 7. Checkpoint - Ensure all tests pass
⦁	Ensure all tests pass, ask the user if questions arise.
⦁	[x] 8. Write unit tests for DOM structure
⦁	[x] 8.1 Set up Jest testing environment
⦁	Install Jest and jsdom dependencies
⦁	Configure Jest for DOM testing
⦁	[x] 8.2 Write unit tests for required elements
⦁	Test hero section elements exist
⦁	Test project cards have required content
⦁	Test contact links are present
⦁	Test meta tags are correct
⦁	Requirements: 1.1-1.6, 2.2-2.5, 4.1-4.3, 7.3
⦁	[x] 9. Final Checkpoint - Ensure all tests pass
⦁	Ensure all tests pass, ask the user if questions arise.
