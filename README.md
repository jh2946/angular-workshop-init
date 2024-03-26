# angular-workshop-init

## Knowing our goal

Download the `prototype` folder as a zip file and place it in a new folder and unzip. This is our wireframe where we'll be copying HTML and CSS, but adding our own functionality using Angular.
- `index.html` contains the wireframe for the task list.
- `about.html` is just a text-only page.

### Additional functionalities
- The tasks should turn translucent when ticked, indicating that it's completed.
- When the "X" is pressed, the tasks should be deleted away.
- When a user types in the title and description and clicks "Create", a new task should be added to the list.
- When a user reloads, all tasks should still be in the same state.

This effectively implements the Create, Read, and Delete functionalities with persistence.

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
        title: 'First task',
        description: 'My first task!',
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

In `app.component.html`, put the following below the navbar:
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

## Task complete / incomplete

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
imports: [NgClass]
```

Do the ES6 import accordingly:
```ts
import { NgClass } from '@angular/common';
```

Save ALL files and observe what happens when you click the task checkbox in the browser window.

## Adding new tasks

Go to `home.component.ts` and add the following to `HomeComponent`:

```ts
addTask(title: string, description: string) {
    this.tasklist.push({
        title,
        description,
        isChecked: false
    });
}
```

This adds a new non-completed task to our task list, with the title and description as function parameters. This should be run every time the user clicks the "Create" button.

We want to grab the value of both inputs. To do this, change the input to:
```html
<input #titleInput type="text" placeholder="New task">
```
and the textarea to:
```html
<textarea #descriptInput rows="5" placeholder="description"></textarea>
```

The `#titleInput` and `#descriptInput` are internal identifiers that help Angular grab values from. Hence we're able to feed them into the event binding to execute `addTask` with the appropriate arguments:

```html
<button (click)="addTask(titleInput.value, descriptInput.value)">Create</button>
```

Save ALL files and try to create a new task in the browser. Something's missing, we'll fix that in the next step.

## Getting the title and description right

We need to pass data from the parent `HomeComponent` into the child `TaskComponent`, hence the use of `@Input`.

In `task.component.ts`, delete the `isChecked = false;` line we added earlier. Replace it with:

```ts
@Input() id = -1;
@Input() title = '';
@Input() description = '';
@Input() isChecked = false;
```

Do the ES6 imports accordingly:

```ts
import { Component, Input } from '@angular/core';
```

In `home.component.ts`, replace `<app-task />` with:

```html
<app-task
[id]="id"
[title]="task.title"
[description]="task.description"
[isChecked]="task.isChecked"
/>
```

Since `tasklist` is the entire list of data of the tasks, `task` represents data for a single task. This data is being fed into `TaskComponent` as "inputs" or "parameters".

In `task.component.html`, make the following changes:

- `History Homework` becomes `{{ title }}` (interpolation)
- `Prelim Paper 1 2022` becomes `{{ description }}`

Save ALL files and observe what happens when you try to add a new task. It works!

We no longer have a need for a placeholder task. Remove the "First task" object from `tasklist` in `home.component.ts` to empty it.

## Deleting a task

The delete button is embedded in each `TaskComponent`, but the data for the task list is in `HomeComponent`. We need `TaskComponent` to signal to `HomeComponent` which task to delete.

This pattern is child-to-parent, hence let's use `@Output` with an `EventEmitter`.

In `task.component.ts`, do:

```ts
@Output() deleteEvent = new EventEmitter();
```

Create a new function:

```ts
deleteItem() {
    this.deleteEvent.emit({
        id: this.id
    });
}
```

The event hence also carries internal data about the task's ID (first task = 0, second task = 1, ...)

Let's go over to the parent's side. In `home.component.ts` do:

```ts
delete(data: any) {
    this.tasklist.splice(data.id, 1);
}
```

This deletes the task in `tasklist`.

Functions that receive `@Output` events may receive data through the first argument. We passed `id` (a number) through this event, hence `data.id` is a number.

To link up the child's `deleteEvent` and the parent's `delete()` function, we do an event binding in `home.component.html`:

```html
<app-task
[id]="id"
[title]="task.title"
[description]="task.description"
[isChecked]="task.isChecked"
(deleteEvent)="delete($event)"
/>
```

And lastly, to get the "X" button click to feed into this whole chain reaction, we do an event binding in `task.component.html`:

```html
<button class="task-delete" (click)="deleteItem()"><img src="assets/close.png" alt=""></button>
```

Save ALL files and try to create a task, then delete it. It works!

## Persistence

If you reload the page, all the tasks you've created will disappear. We'll be using localStorage for persistence to mitigate this issue.

`ngOnInit` is a special class method that gets called when the page is loaded.

We'll be creating the following methods:

- `ngOnInit`: retrieve data from localStorage into app
- `save`: update data from app into localStorage

In `home.component.ts`, paste the following code in `HomeComponent`:

```ts
ngOnInit() {
    this.tasklist = JSON.parse(localStorage['tasklist']);
}

save() {
    localStorage['tasklist'] = JSON.stringify(this.tasklist);
}
```

Add `save` to each method that changes `tasklist`, since writing `save` like this doesn't do anything itself:

```ts
addTask(title: string, description: string) {
    this.tasklist.push({
        title,
        description,
        isChecked: false
    });
    this.save();
}

delete(data: any) {
    this.tasklist.splice(data.id, 1);
    this.save();
}
```

Save ALL files, create a few tasks, reload, delete a few tasks, reload, check a few tasks, reload. Note what you observe.

Our parent has all the persistence methods, but hasn't been informed about checkbox clicks. The `tasklist` in `HomeComponent` hasn't even been updated when a child is checked/unchecked, so let's link it up with another `@Output`.

In `task.component.ts` add a new:

```ts
@Output() changeEvent = new EventEmitter();
```

Since `doneOrNot` is called upon a click of the checkbox, we tack this event on:

```ts
doneOrNot(event: any) {
    this.isChecked = event.currentTarget.checked;
    this.changeEvent.emit({
        id: this.id,
        isChecked: this.isChecked
    });
}
```

This event carries internal data about the task's ID and the state of the checkbox.

In `home.component.ts` we define the new function that will run when this event triggers:

```ts
update(data: any) {
    const id = data.id;
    const updatedTask = this.tasklist[id];
    updatedTask.isChecked = data.isChecked;
    this.save();
}
```

This updates `tasklist` about any checks/unchecks, hence our `tasklist` is now fully up-to-date with our children at all times.

Again, note that since `id` and `isChecked` were passed via the event, we have `data.id` and `data.isChecked` in `update`.

In `home.component.html` we link this `changeEvent` to the `update` function:

```html
<app-task
[id]="id"
[title]="task.title"
[description]="task.description"
[isChecked]="task.isChecked"
(changeEvent)="update($event)"
(deleteEvent)="delete($event)"
/>
```

After all this is done you'll still see an error. Change `ngOnInit` to:

```ts
ngOnInit() {
    if (typeof window !== 'undefined')
        this.tasklist = JSON.parse(localStorage['tasklist']);
}
```

Save ALL files, create a few tasks, check a few boxes, reload. It works!

## Routing

Stop the terminal process and run:
```bash
ng g c task
ng serve --open
```

Go to `prototype/about.html`, copy the inside of `div.content`, paste into `about.component.html`.

In `app.component.html`, replace `<app-home />` with:

```html
<router-outlet />
```

In `app.routes.ts` replace the `routes` array with:

```ts
export const routes: Routes = [
    { path: '', component: HomeComponent },
    { path: 'about', component: AboutComponent }
];
```

Remember the ES6 imports:

```ts
import { HomeComponent } from './home/home.component';
import { AboutComponent } from './about/about.component';
```

In `app.component.ts` put `RouterOutlet` into `imports`:

```ts
imports: [RouterOutlet]
```

Do the ES6 import accordingly:

```ts
import { RouterOutlet } from '@angular/router';
```

Change the navbar hrefs in `app.component.html` accordingly:

```html
<!-- navbar; common across all pages -->
<header class="navbar">
    <a href="/" class="navlink"><h2>To-do List</h2></a>
    <a href="/about" class="navlink"><h3>About Us</h3></a>
</header>
```

Save ALL files, view browser, click "About" in the navbar, then click "To-do List". We now have two pages!

Close VSCode, go to File Explorer, name your folder "My First Angular Project" and drag it onto your desktop (optional)
