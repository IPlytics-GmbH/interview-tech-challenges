# IPlytics Frontend Challenge

Thank you for your interest in becoming part of the IPlytics team.

We are curious about you, your special talents, we want to know how you approach
and solve problems.

Therefore we kindly ask you to show us your approach to solve our little
challenge:

We ask you to implement a minimalist prototype for a management software.

## Requirements
* The minimal version should be able to fetch and display a list of developers 
and a list of projects from the backend
* It should be possible to assign and unassign developer to projects.
* It should be possible to activate and deactivate employees.
* It should be possible to display the estimated end date for a given project
* A dashboard page where we can see [a good plus if you manage time to add those]:
    - A D3 bar chart with project names and count of the features (a nice perk here if you make the count [y axis] changegable so we could switch that same chart to count of developers per projects instead of count of features per project)
    - A D3 pie chart with percentage of projects that are at risk of not making the deadline vs the ones that will

### Further information and constraints:
* One project has a certain number of features.
* For the end date you can assume that the work begins on the current date.
* The number of actual calender days till a project is finished is computed by 
dividing the estimated workdays required by the number of employees assigned 
to it.
* A developer can be either level *senior* or level *junior*.
  * Juniors are assignable to only one project and can implement one feature 
  per day.
  * Seniors are able to work on two projects simultaneously and implement one 
  feature per day and project. If a *senior* developer is only assigned to one 
  project they are able to implement two features in one day.
* To simplify the task, you can assume that the number of features for each 
project is always the same.

### Testing
You don't have to write tests for the complete application but we ask you to 
write one test, that tests some logic of the application and one test that 
tests an actual component.

### Styling
The project does not have to be styled. But make sure the UI is understandable / Usable please!

## Example
Project A - 10 features
Project B - 10 features

If a senior developer is assigned to both A and B projects, then the end date
for both A and B projects will be in 10 days. To simplify the task, you can 
assume that the number of features for each project is always the same.

## Tech Stack:
* Please use ReactJs for the frontend
* Please use ExpressJs for the Backend


## What we are looking for:
* consistent commit history
* clean and well readable code
* iterative approach (we need to able to read through your commit history and see how you iterated on your logic / ui / implementation)
* Well documented readme.md including steps to run your app locally on our side.

## Misc
If you want to show your special talent, you are free to have some extra fun in 
an area of choice. Maybe you want to write some integration tests, are a css 
wizard or pay extra attention to accessibility. It is absolutely fine, to stick 
to the requirements but maybe you want to show us where you shine the most.