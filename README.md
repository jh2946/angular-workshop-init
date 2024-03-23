# angular-workshop-init

## Knowing our goal

Download the `prototype` folder as a zip file and place it in a new folder and unzip. This is our wireframe where we'll be copying HTML and CSS, but adding our own functionality using Angular.
- `index.html` contains the wireframe for the task list.
- `about.html` is just a text-only page.

### Additional functionalities
- The tasks should turn translucent when ticked, indicating that it's completed.
- When the "X" is pressed, the tasks should be deleted away.
- When a user types in the title and description and clicks "Create", a new task should be added to the list.

This effectively implements the Create, Read, and Delete functionalities.

## Starting the project

Terminal:
```bash
ng new project
cd project
```

To start or restart the application, run:
```bash
ng serve --open
```

## Building the navbar

Lines 154 - 158 of `prototype/index.html` contain this HTML:
```html
<!-- navbar; common across all pages -->
<header class="navbar">
    <a href="#" class="navlink"><h2>To-do List</h2></a>
    <a href="#" class="navlink"><h3>About Us</h3></a>
</header>
```
Paste into `app.component.html`. Save.

Lines 16 - 46 of `prototype/index.html` contain this CSS:
```css
/*  navbar styles  */
header.navbar {
    background-color: #ddf;
    display: flex;
    height: 60px;
    padding: 0 5vw;
    border-bottom: 2px black solid;
}
header.navbar a.navlink {
    display: flex;
    box-sizing: border-box;
    text-decoration: none;
    color: inherit;
    height: 100%;
    padding: 15px;
    margin: 0 5px;
    justify-content: center;
    align-items: center;
}
header.navbar a.navlink.right-sep {
    margin-left: auto;
}
header.navbar a.navlink * {
    margin: 0;
}
header.navbar a.navlink:hover {
    background-color: #00f3;
}
div.content {
    padding: 100px 15vw;
}
```
Paste into `app.component.css`. Save and look at the browser after the reload.

Lines 9 - 13 of `prototype/index.html` contain this CSS:
```css
/* everything */
html, body {
    margin: 0;
    font-family: Lato;
}
```
Paste into `project/src/styles.css`. Save and observe the browser after the reload.

## Starting the tasklist (HomeComponent)

Stop the terminal process and run:
```bash
ng g c home
ng serve --open
```

Take note of the `home` directory created.

In `home.component.ts`, paste into `HomeComponent` the following:
```ts
tasklist: any[] = [
    {
        title: 'Task',
        description: 'Description',
        isChecked: false
    }
];
```
This initializes the tasklist, the `any[]` indicating that this array can contain elements of any type. The object inside contains data about the task's title, description, and whether it's checked (completed), but it's just a sample which we'll be removing at the end of the workshop.

In `home.component.html`, open a `<div class="tasklist">`. Then paste the following into this div (copied from `prototype/index.html` lines 184 - 190):

```html
<div id="create">
    <div id="title-input-row">
        <input type="text" placeholder="New task">
        <button>Create</button>
    </div>
    <textarea placeholder="description"></textarea>
</div>
```

In `app.component.html`, type the following:
```html
<div class="content">
    <app-home />
</div>
```

You should see an error, so go to `app.component.ts` and fill the `imports` array with the appropriate component:

```ts
imports: [HomeComponent]
```

and put the appropriate ES6 import:
```ts
import { HomeComponent } from './home/home.component';
```

Save ALL files and observe the changes in the browser window.

Right above this, still in the `<div class="tasklist">`, open a new `<div>` with the following:
```html
<div *ngFor="let task of tasklist; let id = index;">
</div>
```

This creates an HTML element for each object in `tasklist` we created earlier within the class. Additionally, each HTML element has access to the task data (`title`, `description`, `isChecked`) and the position of the object in the array (`id`).

However, you'll get an error. We'll need to import `ngFor`, so go back to `home.component.ts` and in `imports` in line 7, type:

```ts
imports: [NgFor]
```

Do the ES6 import accordingly:

```ts
import { NgFor } from '@angular/common';
```

Stop the terminal process and run:
```bash
ng g c task
ng serve --open
```

Take note of the `task` directory created.

Go back to the ngFor div we created earlier. Put an `<app-task />` inside:
```html
<div *ngFor="let task of tasklist; let id = index;">
    <app-task />
</div>
```

This will throw an error. Go back to `home.component.ts` and `TaskComponent` into `imports`:

```ts
imports: [TaskComponent, NgFor]
```

Do the ES6 import accordingly:
```ts
import { TaskComponent } from '../task/task.component';
```

In `task.component.css`, paste the following (copied from `prototype/index.html` lines 93 - 147):

```css
/* task styles */
div.task {
    border: 2px black solid;
    box-sizing: border-box;
    padding: 30px;
    background-color: #ddf;
    width: min(600px, 60vw);
    margin: 0 auto;
}
div.task {
    margin-top: 30px;
}
div.task * {
    margin: 0;
}
div.task hr {
    margin-top: 5px;
    margin-bottom: 10px;
}
div.task div.top-row {
    display: flex;
    justify-content: space-between;
    height: 30px;
    align-items: center;
}
div.task div.button-row {
    height: 30px;
    align-items: center;
    display: flex;
    box-sizing: border-box;
    border: none;
}
div.button-row > * {
    height: 70%;
    display: block;
    margin-left: 10px;
    cursor: pointer;
}
div.task input[type="checkbox"].task-checkbox {
    aspect-ratio: 1/1;
}
div.task button.task-delete {
    border: none;
    padding: 0;
    background-color: transparent;
}
div.task button.task-delete img {
    height: 100%;
}
div.task a.edit-link {
    text-decoration: none;
}
div.task.completed {
    opacity: 50%;
}
```

In `task.component.html`, paste the following (copied from `prototype/index.html` lines 163 - 172):

```html
<div class="task">
    <div class="top-row">
        <h2>History Homework</h2>
        <div class="button-row">
            <button class="task-delete"><img src="assets/close.png"></button>
            <input class="task-checkbox" type="checkbox">
        </div>
    </div>
    <hr>
    <p>Prelim Paper 1 2022</p>
</div>
```

Drag `prototype/assets/close.png` into `project/src/assets`. It should look like a red cross (our close button).

Save ALL files and observe the changes in the browser window.

In `task.component.ts` type the following into `TaskComponent`:
```ts
isChecked = false;

doneOrNot(event: any) {
    this.isChecked = event.currentTarget.checked;
}
```

In `task.component.html` replace the checkbox input with:
```html
<input class="task-checkbox" type="checkbox" [checked]="isChecked" (click)="doneOrNot($event)">
```

This updates the internal variable `isChecked` every time the checkbox is toggled.

To make the task change visually, we need to toggle a CSS class for the entire TaskComponent itself. This class is `completed`, which has been created for you in one of the above steps in `task.component.css`.

This is done using:
```html
<div class="task" [ngClass]="{'completed': isChecked}">
```
Replace line 1 of `task.component.html` with this.

This will throw an error. In `task.component.ts`, insert `NgClass` into `imports`:
```ts
imports: [NgClass, RouterLink]
```

Do the ES6 import accordingly:
```ts
import { NgClass } from '@angular/common';
```

Save ALL files and observe what happens when you click the task checkbox in the browser window.
