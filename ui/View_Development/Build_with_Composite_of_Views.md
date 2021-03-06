#Build with Composite of Views

The simplest way to implement a view is to compose it with other views like a macro view (aka., a composite view). For example, assume we want to have a view that displays a label and also an input as shown below.

![A Composite View](composite.jpg?raw=true)

> The source code can be found at github: [composite-view](source:example).

##Composite

You can implement it by extending from [View](api:view) or any of its subclasses as follows.

    class LabeledInput extends View {
      TextView _label;
      TextBox _input;

      LabeledInput(String label, [String value]) {
        layout.type = "linear"; //use horizontal linear layout
        addChild(_label = new TextView(label));
        addChild(_input = new TextBox(value));
      }
      String get label => _label.text;
      void set label(String label) {
        _label.text = label;
      }
      String get value => _input.value;
      void set value(String value) {
        _input.value = value;
      }
    }

Then, the application can use it without knowing how it is implemented.

    new View()
      ..layout.text = "type: linear; orient: vertical"
      ..addChild(new LabeledInput("username"))
      ..addChild(new LabeledInput("password"));

> If you're familiar with XML, you can use [Rikulo UXL](../UXL) to composite the views. Please refer to [Use UXL Templates in Dart](../UXL/Fundamentals/Use_UXL_Templates_in_Dart.md) for more information.

##Event Handling

You can *propagate* the events sent by the internal views to the caller of the composite view. To do so, you can listen to the event you care and handle it the way you want. For example, in this sample, you can propagate the `change` event as follows.

    LabeledInput(String label, [String value]) {
      ...
      _input.on.change.add((ChangeEvent event) {
        sendEvent(new ChangeEvent(event.value, target: this));
        });
    }

Then, the application can listen the `change` event if necessary.

    for (LabeledInput view in rootView.queryAll("LabeledInput"))
      view.on.change.add((event) {
        print("$event received");
        });
