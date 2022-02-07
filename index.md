## Filippo Cortese
### Portfolio

#### Index
1. GraphIt - Unity Tool
2. Character Setup - Unity Tool (_Under development_)
3. The Gaia Project - Game on Itch
4. Cypherogue - Game on Itch
5. Digital card game - AI practice

#### GraphIt - Unity Tool
GraphIt is a Unity tool that allows developers to display graphs in the unity editor directly. This is especially useful if the developers want to visualize large arrays full of data only in the editor, without losing any performance in the final/development build. This can be handy with regards to balancing, spotting of major bugs and triggers to these bugs, errors in underlying logic, etc... It is not aimed to be a replacement with regards to the recording of historical data and post-playtest data analysis, but rather as an in-engine playtest data debugger/visualizer.

##### Usage
After importing the `.unitypackage`, using GraphIt is very simple:
- Firstly, create a new C# script and attach it to a `GameObject`;
- add the inclusion `using Graphing` at the top of your script;
- In your newly created script, create a global variable of type `Graph`, but do not instantiate it:
  ```
  Graph graph;
  ```
- Call the following method once when you want to create a graph in the inspector:
  ```
  GraphIt.CreateGraph(GameObject pointToObject, out Graph graph);
  ```
  _pointToObject: specify what GameObject's inspector you want the graph to appear on. More often than not, you will set this to `this.gameObject`_
  _graph: specify what reference of Graph you want to create. This is the same obejct you will reference later to set data and edit style_
  For example:
  ```
  Graph graph;
  
  private void Start()
  {
      GraphIt.CreateGraph(gameObject, out graph);
  }
  ```
