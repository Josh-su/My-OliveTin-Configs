# The Final Product
![image](https://github.com/user-attachments/assets/84a5c89a-eace-41bb-bac6-aa42ca3f3dca)

## Explanation

### YAML Configuration
#### This section outlines the main configuration settings for your OliveTin setup
```yaml
logLevel: "INFO"
showFooter: false
showNavigation: true
sectionNavigationStyle: topbar
themeName: CatppuccinMocha
pageTitle: YourPageTitle
```
These parameters control various aspects of the application, such as log verbosity, navigation visibility, and the selected theme.

#### Action to Update the Container Filtered List
```yaml
actions:
  - title: Update container entity file
    shell: docker ps -a --filter "name=ContainerName" --filter "name=Container2Name" --format json > /path/to/container.json
    hidden: true
    execOnStartup: true
    execOnCron: '*/5 * * * *'
```
This action updates the entity file containing the list of containers. You can add a new --filter flag for each container you want to include. By using a nomenclature for your container names, you can filter them more efficiently. The filtering is done using the name= parameter, but you can use other options to tailor your query further. This allows you to create a concise filtered list before writing to the entities file.

#### Example Actions
```yaml
  - title: Start {{ container.ID }}
    icon: <iconify-icon icon="tabler:play"></iconify-icon>
    shell: docker start {{ container.Names }}
    entity: container
    trigger: Update container entity file

  - title: Stop {{ container.ID }}
    icon: <iconify-icon icon="mingcute:stop-line"></iconify-icon>
    shell: docker stop {{ container.Names }}
    timeout: 8
    entity: container
    trigger: Update container entity file
```
These examples define actions for starting and stopping containers. Each action specifies an icon, the shell command to execute, and the entity it relates to. Both actions trigger the update of the container entity file to reflect the latest status.

#### The Entities
```yaml
# Docs: http://docs.olivetin.app/entities.html
entities:
  - file: /path/to/container.json
    name: container
```
This section specifies the entity file containing the filtered list of containers. It serves as the data source for the dashboard.

#### The Dashboard with Entities and ```cssClass```
```yaml
dashboards:
  - title: Containers
    contents:
      - title: |
          Container {{ container.Names }}
          Image Source: {{ container.Image }}
        entity: container
        type: fieldset
        contents:
          - type: display
            title: |
              <h1>Status</h1>
              <p class="{{ container.State }}">{{ container.Status }}</p>
          - title: 'Start {{ container.ID }}'
            cssClass: '{{ container.State }}'
          - title: 'Stop {{ container.ID }}'
            cssClass: '{{ container.State }}'
```
This section outlines the dashboard configuration for displaying container information. The cssClass is dynamically assigned based on {{ container.State }} for each action and status display, allowing for contextual styling. This enables users to see which actions are available based on the current state of the container (e.g., exited or running).

---

### CSS 
Utilizing the custom theme [Catppuccin Mocha](https://www.olivetin.app/themes/posts/catppuccinmocha/), I made the following additions:

#### Hide Unnecessary Elements
```css
section[system-title="Containers"][title="Containers"] span.title,#sidebar-toggler-button,.userinfo {
    display: none;
}
```
This CSS rule hides specific elements in the UI that are not needed for my configuration, such as the title of the containers section, the sidebar toggler button, and user information.

#### Control Button Activation and Deactivation
```css
/* General deactivation style */
action-button.running button[title*="Start"],
action-button.exited button[title*="Stop"] {
    pointer-events: none;
    opacity: 0.5; /* optional: make inactive buttons appear disabled */
}

/* General activation style */
action-button.exited button[title*="Start"],
action-button.running button[title*="Stop"] {
    pointer-events: auto;
    opacity: 1;
}
```
These CSS rules manage the state of action buttons based on the container's status. Buttons appear disabled and semi-transparent when inactive, while they are fully interactive when the corresponding container is running.

#### change status color by the container.state that change the css class
```css
p.running {
  color: green;
}

p.exited {
  color: red;
}

p.created {
  color: yellow;
}
```
Change Status Colors Based on Container State

