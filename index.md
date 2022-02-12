## Filippo Cortese

#### Index
1. GraphBrain - Unity Tool
2. Character Setup - Unity Tool (_Under development_)
3. The Gaia Project - Game on Itch
4. Cypherogue - Game on Itch
5. Digital card game - AI practice


#### GraphBrain - Unity Tool
GraphBrain is a Unity tool that allows developers to display graphs in the unity editor directly. This is especially useful if the developers want to visualize large arrays full of data only in the editor, without losing any performance in the final/development build. This can be handy with regards to balancing, spotting of major bugs and triggers to these bugs, errors in underlying logic, etc... It is not aimed to be a replacement with regards to the recording of historical data and post-playtest data analysis, but rather as an in-engine playtest data debugger/visualizer.

##### Usage
After importing the `.unitypackage`, using GraphBrain is very simple:
- Firstly, create a new C# script and attach it to a `GameObject`;
- add the inclusion `using Graphing` at the top of your script;
- In your newly created script, create a global variable of type `Graph`, but do not instantiate it:
  ```csharp
  Graph graph;
  ```
- Call the following method once when you want to create a graph in the inspector:
  ```csharp
  GraphBrain.CreateGraph(GameObject pointToObject, out Graph graph);
  ```
  _pointToObject: specify what GameObject's inspector you want the graph to appear on. More often than not, you will set this to `this.gameObject`_
  
  _graph: specify what reference of Graph you want to create. This is the same obejct you will reference later to set data and edit style_
  
  For example:
  ```csharp
  Graph graph;
  
  private void Start()
  {
      GraphBrain.CreateGraph(gameObject, out graph);
  }
  ```
 
- Now, it is up to the developer to chose what graph type  of visualizer you need:
  1. Represent a set of data of the same length at all times (e.g. representing how many people fall within every age between 0-100 in a bar graph)
  2. Log and track display and trend of gametime variables (e.g. in a turn based game, displaying player gold for the current turn and last 6 turns, updated in real-time)

- In both cases, you set the graph's values using the following method:
  ```csharp
  graph.SetValues(GraphValue[] points, GraphStyle graphStyle);
  ```
  where `points` is an array of GraphValues (which include a `float Value` and `string Tag`) to plot and a GraphStyle which is used as a drawing visual style for the graph. 
  To get an array of GraphValue[], a handy static method was created that takes in a `string[]` of tags (labels for the X axis of each value) and a `float[]` of values to  display. The method is as follows:
  ```csharp
  GraphBrain.FormatData(float[] values, string[] tags);
  ```

  For example, we can create and display values as follows:
  ```csharp
  float[] values = new float[5] {1, 2, 3, 4, 5};
  string[] labels = new string[5] {"a", "b", "c", "d", "e"};

  Graph graph;

  private void Start()
      {
          GraphBrain.CreateGraph(gameObject, out graph, "Test Graph");

          graph.SetValues(GraphBrain.FormatData(values, labels), GraphStyle.DefaultPredict);
      }

  ```
  
##### Graph Styles
GraphStyle is a struct that allows the developer to specify parameters around which the graph is displayed.
The struct can be used to define the following, and every field must be specified when not using a default static style that comes already defined in the package:
| Field | Data Type | Description |
| --- | --- |
| bottomMargin | float | Size of bottom margin for X axis labels |
| leftMargin | float | Size of left margin for Y axis labels |
| axisWidth | float | Thickness of X and Y axis lines |
| graphAreaHeight | float | Height of graph area in pixels. I recommend at least 120px |
| yAxisSteps | int  | Number of tags/steps on the y axis labeling |
| canSwitchToOtherTypes | bool | States whether graph can be changed to line or bar graph in the inspector |
| scaleToDesired | bool | Whether the graph should fit a specific Y axis maximum value |
| fitToYval | int | Value Y axis should be fitted to |
| yAxisFormatting | string  | Type of formatting to Y axis values, as accepted by .ToString(), such as F0, F2, etc... |
| yAxisUnitSign | string | Unit sign/symbol to append to Y axis labels. Leave empty for only numerical values |
| lineGraphPredict | bool | Wether you want the line graph view to show trend and approximate prediction based on given data. |
| lineGraphPredictionSpace | float | What percentage of horizontal visual space should be dedicated to the prediction |
| liveItemAdd | bool | Whether the graph should record the last value(s) passed to it through .SetValues() is memory |
| liveItemBuffer | int | Number of values that are allowed to be recorded and backlogged for Live Item Add |


