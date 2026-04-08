extract the .exe file 
goto /res/scenes/FlagLabel.cs 
```
using Godot;

public class FlagLabel : Label
{
	private string HiddenContent = "";

	private int[] SomethingNotInterestingAtAll = new int[31]
	{
		71, 71, 44, 32, 116, 104, 101, 32, 102, 108,
		97, 103, 32, 105, 115, 10, 67, 35, 49, 115,
		51, 52, 115, 89, 84, 48, 98, 82, 51, 52,
		107
	};

	public override void _Ready()
	{
		int[] somethingNotInterestingAtAll = SomethingNotInterestingAtAll;
		foreach (int code in somethingNotInterestingAtAll)
		{
			HiddenContent += (char)code;
		}
		base.Text = "How did you\ndo that?!";
	}
}
```
we need to write them in utf-8
