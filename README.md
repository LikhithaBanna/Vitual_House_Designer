# Smart House Planner

A modern web app for planning a house in **2D** and visualizing it in **3D**.
Built with React, TypeScript, Vite, React Konva and React Three Fiber.
All data is saved in your browser (Local Storage), so no backend or database is needed.

## Quick start

You need [Node.js](https://nodejs.org) version 18 or newer.

```bash
npm install
npm run dev
```

Open the address Vite prints (usually http://localhost:5173).

Other commands:

| Command | What it does |
|---|---|
| `npm run dev` | Starts the development server |
| `npm run build` | Type-checks and creates a production build in `dist/` |
| `npm run preview` | Serves the production build locally |

## How to use it

1. **Register** an account (stored only in your browser) and log in.
2. Click **Create New Project**. Enter the name, plot width and length, unit (feet or meters) and number of floors.
   Leave "Add starter rooms" ticked to get a ready-made layout from your bedroom/bathroom choices.
3. In the **2D Plan** editor:
   - Use the **Add Room / Bedroom / Bathroom ...** buttons to add rooms.
   - Drag a room to move it, drag its handles to resize it. Rooms snap to the grid and cannot leave the plot.
   - **Add Door / Add Window**: pick the tool, then click next to a room wall. They snap to walls and can be dragged along them. Windows can be resized with their side handles.
   - **Add Wall**: click and drag to draw a straight interior wall.
   - Edit exact numbers (name, width, height, X, Y, color, rotation) in the **Properties** panel on the right.
   - Press `Delete` to remove the selected object and `Esc` to deselect.
   - On a multi-floor plot, switch floors with the floor buttons at the top.
4. Open the **Interior** tab. Click a furniture item (or drag it onto the plan) to add it. Move it, resize it and rotate it with the handles or the Properties panel.
5. Open the **Room Photo** tab to try furniture on a real room: upload a photo (or drag one in), click furniture to place it on the photo,
   then drag, resize (corner handles), rotate, recolor, flip or duplicate it. Use **Download picture** to save the result as a JPG.
   The photo is shrunk and saved inside the project (in this browser).
6. Open the **3D View** tab. Left-drag to rotate, right-drag to pan, scroll to zoom.
   Switch between **Perspective** and **Top view**, and toggle walls or furniture.
7. Open the **Preview** tab for the project summary. From there you can **Save**, **Export** as JSON, **Edit** or **Delete** the project.
   Changes are also auto-saved every time you edit.

## Folder structure

```
src/
├── components/
│   ├── common/      reusable UI (form fields, dialogs, project card, buttons)
│   ├── layout/      navbar, footer, dashboard layout, editor header, route guards
│   ├── floorplan/   2D editor: canvas, shapes, toolbars, properties panel
│   └── three/       3D scene: house, furniture models, camera and lights
├── pages/           one folder per page (Home, Login, Register, Dashboard, ...)
├── store/           Zustand stores (auth, projects, editor state)
├── hooks/           reusable hooks (element size, plan actions, shortcuts)
├── types/           TypeScript interfaces (User, Project, Room, Door, ...)
├── utils/           geometry, units, validation, 3D wall builder, storage, export
├── data/            landing page content, room types, furniture catalog
├── assets/          images (logo)
└── styles/          global, app and editor CSS
```

## How the data works

Everything about a house is one `Project` object:

```
Project
 ├── details (name, dates)
 ├── plot (width, length, unit, floors)
 └── plan
      ├── rooms
      ├── doors
      ├── windows
      ├── walls
      └── furniture
```

Positions and sizes are stored in the project's own unit (feet or meters), **not** pixels, so the same data drives the 2D editor, the 3D view and the preview.

## Adding a backend later

Only `src/utils/storage.ts` talks to Local Storage. To use a real API, replace the functions there
(and make the stores call them with `await`). Pages and components do not need to change.

## Notes and limits

- Accounts and projects live only in this browser. Clearing site data deletes them. Use **Export Project** to keep a backup.
- Password hashing here is for learning only. A real app must hash passwords on a server.
- Rooms are rectangles. The "Rotate 90°" button on a room swaps its width and height; free rotation is available for furniture, doors and windows.
- Doors and windows only cut openings in walls that run along a room edge or a drawn wall.
- The Room Photo tab places simple front-view furniture drawings on your photo. It does not detect the room or match perspective, so use it to compare size, color and style. Photos are compressed to fit browser storage; very large photos or many projects can fill it up (you will see a message).
- The 2D editor is built for desktop. On small screens the panels stack above and below the canvas.

## Troubleshooting

- **`npm install` fails**: check your Node version with `node -v` (needs 18+).
- **Blank 3D view**: your browser needs WebGL enabled (hardware acceleration on).
- **Lost projects**: they are tied to the browser and address (`localhost:5173`) you used.
