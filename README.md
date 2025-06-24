# 🎸 Brakrock Festival Planner

A Vue.js application for planning your perfect day at the Brakrock festival. Drag and drop bands from the timetable to create your personalized schedule with automatic overlap detection.

## Features

- **📅 Complete Festival Timetable**: All bands from Woodstage, Riverstage, and Lakestage
- **🎨 Color-Coded Stages**: Each stage has a distinct color for easy identification
- **🖱️ Drag & Drop Interface**: Simply drag bands from the timetable to your planner
- **⚠️ Overlap Detection**: Automatically detects and highlights conflicting performances
- **📱 Responsive Design**: Works perfectly on desktop and mobile devices
- **🗑️ Easy Management**: Remove bands from your planner with a single click

## Getting Started

### Prerequisites

- Node.js (version 14 or higher)
- npm or yarn

### Installation

1. Clone or download this project
2. Navigate to the project directory
3. Install dependencies:

```bash
npm install
```

### Running the Application

Start the development server:

```bash
npm run dev
```

The application will open automatically in your browser at `http://localhost:3000`

### Building for Production

To create a production build:

```bash
npm run build
```

## How to Use

1. **View the Timetable**: The left side shows all bands organized by stage
2. **Plan Your Day**: Drag bands from the timetable to the "My Day Planner" section on the right
3. **Check for Overlaps**: The app automatically detects and highlights overlapping performances
4. **Manage Your Schedule**: Remove bands you don't want by clicking the × button
5. **Stage Colors**: 
   - 🟫 Woodstage (Brown)
   - 🔵 Riverstage (Blue)
   - 🟢 Lakestage (Green)

## Festival Data

The app includes the complete Brakrock festival timetable for Saturday, August 2, 2025:

- **Woodstage**: 9 bands including Madball, Zeke, and Codefendants
- **Riverstage**: 8 bands including Circle Jerks, Millencolin, and Agnostic Front
- **Lakestage**: 9 bands including NOFX, The Last Gang, and Popes of Chillitown

## Technologies Used

- **Vue.js 3**: Modern reactive framework
- **Vite**: Fast build tool and development server
- **CSS3**: Modern styling with gradients and animations
- **HTML5 Drag & Drop API**: Native browser drag and drop functionality

## Browser Support

This application works in all modern browsers that support:
- ES6+ JavaScript
- CSS Grid and Flexbox
- HTML5 Drag & Drop API

## License

This project is open source and available under the MIT License. 