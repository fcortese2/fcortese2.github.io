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
 
- Now, it is up to the developer to chose what graph type  of visualizer you need:
  1. Represent a set of data of the same length at all times (e.g. representing how many people fall within every age between 0-100 in a bar graph)
  2. Log and track display and trend of gametime variables (e.g. in a turn based game, displaying player gold for the current turn and last 6 turns, updated in real-time)

- In both cases, you set the graph's values using the following method:
  ```
  graph.SetValues(GraphValue[] points, GraphStyle graphStyle);
  ```
  where `points` is an array of GraphValues (which include a `float Value` and `string Tag`) to plot and a GraphStyle which is used as a drawing visual style for the graph. 
  To get an array of GraphValue[], a handy static method was created that takes in a `string[]` of tags (labels for the X axis of each value) and a `float[]` of values to  display. The method is as follows:
  ```
  GraphIt.FormatData(float[] values, string[] tags);
  ```

  For example, we can create and display values as follows:
  ```
  float[] values = new float[5] {1, 2, 3, 4, 5};
  string[] labels = new string[5] {"a", "b", "c", "d", "e"};

  Graph graph;

  private void Start()
      {
          GraphIt.CreateGraph(gameObject, out graph, "Test Graph");

          graph.SetValues(GraphIt.FormatData(values, labels), GraphStyle.DefaultPredict);
      }

  ```
