# CS 340 Portfolio

This repository contains my portfolio submission for CS 340: Client/Server Development.

## Project Two: Grazioso Salvare Rescue Animal Dashboard

An interactive web dashboard built with Dash and MongoDB that helps Grazioso Salvare identify shelter dogs suited for water rescue, mountain/wilderness rescue, and disaster/individual tracking work. The dashboard connects to the animal shelter's MongoDB database through a reusable CRUD Python module (originally built in Project One), and lets a non-technical user filter the full dataset by rescue type, view the matching animals in a sortable/filterable table, and see a breed distribution chart and a geolocation map update live based on the current selection.

**Files in this repository:**
- `ProjectTwoDashboard_PROSPER_ASHANGA.ipynb` — the dashboard application code
- `ProjectTwo_README.docx` — write-up covering the tools used, the development process, and the challenges encountered while building the dashboard

## Project Two Reflection

**How do you write programs that are maintainable, readable, and adaptable?**

The CRUD Python module I built in Project One turned out to be the clearest example of this for me. I wrote it as a single, isolated `AnimalShelter` class with four methods — create, read, update, delete — that do nothing but talk to MongoDB. Every other part of my code, including the entire Project Two dashboard, only ever interacts with the database through that class. When I built the dashboard, I didn't touch the CRUD module at all; I just imported it and called `db.read(query)`. That separation is what maintainability actually looks like in practice: when something went wrong during testing, I knew immediately whether the bug was in my dashboard logic or in the database layer, because they were never mixed together in the same file. It also made the module reusable in a way I didn't fully appreciate until I needed it again — the exact same file, unchanged, powered a completely different application. In the future I could drop this same module into a REST API backend, a command-line admin tool, or any other project that needs to read and write this shelter data, without rewriting a single line of database logic.

**How do you approach a problem as a computer scientist?**

For this project, the starting point wasn't code at all — it was the Dashboard Specifications Document, which laid out exactly which breeds, sex, and age ranges Grazioso Salvare wanted for each rescue type. I translated those business rules directly into MongoDB query filters (`$in` for breed lists, `$gte`/`$lte` for age ranges) rather than guessing at what "a good rescue dog" might mean. I also learned not to trust that the spec and the real data always agree — when I tested the Disaster/Tracking query against the actual collection, "Doberman Pinscher" returned zero results because the dataset stores it as the truncated "Doberman Pinsch." Catching that by testing against real data, instead of assuming the written spec was literally correct, was the difference between a dashboard that looked right and one that actually worked. That's a different mode than most of my other coursework, where the problem and the expected output are both already well-defined. Here I had to interpret a non-technical client's request, verify it against messy real-world data, and build something usable end to end. Going forward, my strategy for any new client database request is the same: read the requirements closely, verify them against the actual data before trusting them, and keep the data-access layer separate from everything else so the system stays easy to debug and extend.

**What do computer scientists do, and why does it matter?**

At the core, computer scientists take raw, messy data and turn it into something people can actually use to make decisions. Before this dashboard, finding a dog that matched a specific rescue profile meant someone manually scanning through a 10,000-row spreadsheet. Now it's a single click on a filter, with a map and a breed chart that make the results immediately understandable. That's not just a convenience — for an organization like Grazioso Salvare, faster and more accurate matching between a rescue type and a candidate dog directly affects how quickly they can respond to a real search-and-rescue need. That's really the point of the work: good software doesn't just process data, it removes friction between people and the information they need to act on, and for a client like this, that friction has real consequences.
