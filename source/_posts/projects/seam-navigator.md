---
title: Seam Navigator
subtitle: 3D Dijkstra React Project
category: projects
permalink : projects/seam-navigator/
cover_image: 'compass_cover.jpg'
header_image: 'compass_post.jpg'
header_image_alt_text: A photo of an old fashioned compass with a blurred map in the background. Photo by Dunamis Church on Unsplash.
tags:
show_date: false
index: 1
---

## Navigation problems

During the pandemic I ran a [D&D game](https://en.wikipedia.org/wiki/Dungeons_%26_Dragons) for a group of my friends. The game was set in a mythical world where airships would fly between different floating cities, towns and villages. This world was known as "The Seam".

Some of the routes between these towns were well established "chalked" routes lit with lights, whilst others were not as well established. Navigating between these non-established routes was far more dangerous due to the perpetual darkness that covered the world of the Seam. Flying like this was known as flying "through the black".

As the game went on, I found that calculating the distances the players were travelling to be pretty time consuming without a tool to do the maths for me. At the same time, I was also interested in trying out React. The result was "[Seam Navigator](https://seam-navigator.callumh.io)", a web tool that finds the shortest path between two locations in the Seam.

## Development

The code for this project can be found on my GitHub at [CallumHewitt/seam-navigator](https://github.com/CallumHewitt/seam-navigator).

### Layout

I used [React](https://react.dev) to create the tool as I'd never played with it before and I wanted to learn how it worked. To keep things simpler on the CSS side, I used [Bootstrap](https://getbootstrap.com) for all of the styling.

Aside from the taskbar heading, I knew that there would be two main parts to the tool:

- The inputs for the different stops.
- The calculated route.

This translated to three React components in the final project:

- `Stop`, consisting of a datalist input, a button for deleting the stop and a button to clear the text input.
- `RouteCard`, a bootstrap card containing the route to get between two stops and the distances involved.
- `Plotter`, the parent component containing the stops and route cards.

### Path Finding

The map is defined in a JSON file. It contains two fields:

- `nodes`: An object which has the X, Y and Z co-ordinates for each location in the Seam.
- `edges`: An array of pairs defining the chalked routes between locations.

The tool needed to support two forms of path calculations. The first method calculates the shortest distance between two locations using chalked routes. I used [Dijkstra's algorithm](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm) to calculate the shortest route, calculating the distances between each node using their relative co-ordinates.

However, this method doesn't cover locations that don't have chalked routes to them. To calculate a path to those locations, the code first finds the chalked route to the node closest to the location using Dijkstra, and then calculates the direct distance to the final location from there.

### Maintaining State

This was my first react project so it took me some experimentation before I found a configuration that I was happy with when it came to maintaining state.

My original design had the route cards and stops maintain their own state. Each `Stop` would update it's state with the selected value, and report the name of a selected node to the `Plotter` via the `onChanged` event. Each `RouteCard` would also do it's own routing calculations based on the nodes provided.

However, this proved to be difficult to manage. One critical bug occurred when deleting a `Stop` from the route if it was in-between two other stops. The route cards would update correctly, calculating the route between the two now-adjacent stops. However, the loop that renders the `Stop` inputs would be unable to update the content of the stop inputs with the new route locations. This led to the stop at the index of the now-deleted stop rendering the input text for the deleted stop instead of it's own.

In the end managing the state of the `Stop` content and the `RouteCard` content entirely within the `Plotter` component fixed this bug and also meant that all of my business logic for calculating the route from a set of stops could be contained within one class instead of three.

I'm still not sure if this is the best way to manage state in React. There might be more effective patterns but I found that, for this app at least, this approach was a good way of maintaining separation of concerns.

## Summary

![A screenshot of the final application showing the stops and the route cards](seam-navigator.png)

The tool is available at [seam-navigator.callumh.io](https://seam-navigator.callumh.io) for anyone to test out.

Overall, this was a really fun project that was helped to make the D&D games run more smoothly when it came to navigation. It also taught me a lot about React and in particular about managing state between components. I would definitely be keen to try out react again in the future on some other projects.
