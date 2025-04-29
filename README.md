# Interview Coder

An invisible desktop application that will help you pass your technical interviews.

https://github.com/user-attachments/assets/caf1e6cd-27d5-4033-b8c5-9df1cb52b21d

## Invisibility Compatibility

The application is invisible to:

- Zoom versions below 6.1.6 (inclusive)
- All browser-based screen recording software
- All versions of Discord
- Mac OS screenshot functionality (Command + Shift + 3/4)

Note: The application is **NOT** invisible to:

- Zoom versions 6.1.6 and above
- Mac OS native screen recording (Command + Shift + 5)

## Features

- 🎯 99% Invisibility: Undetectable window that bypasses most screen capture methods
- 📸 Smart Screenshot Capture: Capture both question text and code separately for better analysis
- 🤖 AI-Powered Analysis: Automatically extracts and analyzes coding problems
- 💡 Solution Generation: Get detailed explanations and solutions
- 🔧 Real-time Debugging: Debug your code with AI assistance
- 🎨 Window Management: Freely move and position the window anywhere on screen

## Global Commands

The application uses unidentifiable global keyboard shortcuts that won't be detected by browsers or other applications:

- Toggle Window Visibility: [Control or Cmd + b]
- Move Window: [Control or Cmd + arrows]
- Take Screenshot: [Control or Cmd + H]
- Process Screenshots: [Control or Cmd + Enter]
- Reset View: [Control or Cmd + R]

## Usage

1. **Initial Setup**

   - Launch the invisible window
   - Authenticate with OpenAI API key

2. **Capturing Problem**

   - Use global shortcut to take screenshots
   - Capture question text and code separately for better analysis
   - Screenshots are automatically added to the processing queue

3. **Processing**

   - AI analyzes the screenshots to extract:
     - Problem requirements
     - Code context
   - System generates optimal solution strategy

4. **Solution & Debugging**

   - View generated solutions
   - Use debugging feature to:
     - Test different approaches
     - Fix errors in your code
     - Get line-by-line explanations
   - Toggle between solutions and queue views

5. **Window Management**
   - Move window freely using global shortcut
   - Toggle visibility as needed
   - Window remains invisible to specified applications
   - Reset view using Command + R

## Prerequisites

- Node.js (v16 or higher)
- npm or bun package manager
- OpenAI API key (for AI features)
- Screen Recording Permission for Terminal/IDE
  - On macOS:
    1. Go to System Preferences > Security & Privacy > Privacy > Screen Recording
    2. Ensure your Terminal app (or IDE) has screen recording permission enabled
    3. Restart your Terminal/IDE after enabling permissions
  - On Windows:
    - No additional permissions needed
  - On Linux:
    - May require `xhost` access depending on your distribution

## Installation

1. Clone the repository:

```bash
git clone https://github.com/ibttf/interview-coder.git
cd interview-coder
```

2. Install dependencies:

```bash
npm install
# or if using bun
bun install
```

## Running Locally

1. Start the development server:

```bash
npm run app:dev
# or
bun run app:dev
```

This will:

- Start the Vite development server
- Launch the Electron application
- Enable hot-reloading for development

## Building for Production

To create a production build:

```bash
npm run app:build
# or
bun run app:build
```

The built application will be available in the `release` directory.

## Tech Stack

- Electron
- React
- TypeScript
- Vite
- Tailwind CSS
- Radix UI Components
- OpenAI API

## Configuration

1. Create a `.env` file in the project root by copying the `.env.example` file.
2. Open the `.env` file and add your OpenAI API key:
   ```
   OPENAI_API_KEY=YOUR_API_KEY_HERE
   ```
3. The application will load this key on startup. Your settings are stored locally using electron-store, but the primary API key management is now via the `.env` file.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.
