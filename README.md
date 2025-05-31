Temperature Converter App
Overview
The Temperature Converter is a Flutter mobile application that allows users to convert between Fahrenheit and Celsius temperature scales. The app features a responsive design that adapts to both portrait and landscape orientations, maintains a history of conversions, and provides precise calculations using standard temperature conversion formulas.

App Screenshot

Key Features
Dual Conversion Modes: Fahrenheit to Celsius and Celsius to Fahrenheit

Precise Calculations: Uses standard conversion formulas with 2-decimal precision

Conversion History: Maintains session history with most recent conversions first

Responsive Design: Optimized layouts for portrait and landscape orientations

Intuitive UI: Clean Material Design interface with clear visual hierarchy

Technical Architecture
Core Components
lib/
└── main.dart
    ├── TemperatureConverterApp (StatelessWidget)
    └── ConverterScreen (StatefulWidget)
        ├── _ConverterScreenState
        │   ├── _convert() - Conversion logic
        │   ├── _buildConversionSelector() - Radio buttons
        │   ├── _buildInputField() - Temperature input
        │   ├── _buildHistoryList() - Conversion history
        │   ├── _buildPortraitLayout() - Vertical layout
        │   └── _buildLandscapeLayout() - Horizontal layout
        └── HistoryItem (Data Model)
Dependencies
Flutter SDK (minimum 3.0.0)

Material Design widgets (included in Flutter)

Critical Components
1. Conversion Logic (_convert method)
dart
void _convert() {
  final input = double.tryParse(_inputController.text);
  if (input == null) return;

  final convertedValue = _conversionType == ConversionType.fahrenheitToCelsius
      ? (input - 32) * 5 / 9   // °F to °C
      : input * 9 / 5 + 32;     // °C to °F

  setState(() {
    _result = convertedValue.toStringAsFixed(2);
    _history.insert(0, HistoryItem(
      type: _conversionType,
      input: input,
      result: convertedValue,
    ));
  });
}
Validates numeric input

Applies correct conversion formula based on selection

Updates result with 2-decimal precision

Adds conversion to history (most recent first)

2. Responsive Layout (OrientationBuilder)
dart
OrientationBuilder(
  builder: (context, orientation) {
    return orientation == Orientation.portrait
        ? _buildPortraitLayout()
        : _buildLandscapeLayout();
  },
)
Dynamically switches between layouts

Portrait: Vertical column layout

Landscape: Horizontal split-screen layout

3. History Management
dart
class HistoryItem {
  final ConversionType type;
  final double input;
  final double result;

  HistoryItem({required this.type, required this.input, required this.result});
}

// In State class
final List<HistoryItem> _history = [];

// In build method
ListView.builder(
  itemCount: _history.length,
  itemBuilder: (context, index) {
    final item = _history[index];
    return ListTile(
      title: Text(
        '${item.type == ConversionType.fahrenheitToCelsius ? 'F to C' : 'C to F'}: '
        '${item.input.toStringAsFixed(1)} → ${item.result.toStringAsFixed(1)}',
      ),
    );
  },
)
HistoryItem model stores conversion data

ListView.builder efficiently renders history

Most recent conversions appear at top of list

4. User Interface Components
Radio Buttons: For conversion type selection

TextField: For temperature input with numeric keyboard

ElevatedButton: Triggers conversion

ListView: Displays scrollable history

VerticalDivider: Separates sections in landscape mode

SizedBox/Spacers: Provides consistent spacing

Getting Started
Prerequisites
Flutter SDK (v3.0 or higher)

Dart SDK (v2.17 or higher)

Android Studio/Xcode (for emulators)

Installation
Clone the repository:

bash
git clone https://github.com/yourusername/temperature-converter-flutter.git
Install dependencies:

bash
flutter pub get
Run the app:

bash
flutter run
Building for Production
Android APK:

bash
flutter build apk --release
iOS App Bundle:

bash
flutter build ios --release
Usage Guide
Select Conversion Type:

Choose between "Fahrenheit to Celsius" or "Celsius to Fahrenheit"

Enter Temperature:

Input a numeric value in the text field

The suffix shows the current input unit (°F or °C)

Convert:

Press the "Convert" button to calculate

Result appears below the button

View History:

Previous conversions appear in the history section

Most recent conversion at the top

Rotate Device:

App automatically adjusts layout for portrait/landscape

Testing
The app includes manual testing procedures for:

Conversion accuracy (known values)

History tracking functionality

Orientation responsiveness

Input validation

UI consistency across orientations

Test cases include:

32°F → 0°C

0°C → 32°F

100°C → 212°F

212°F → 100°C

Invalid input handling

History ordering

Technical Decisions
State Management: Uses setState for simplicity given the app's scope

Responsive Design: OrientationBuilder for layout switching

Data Modeling: HistoryItem class for structured data storage

Input Handling: TextInputType.number with input validation

Precision: toStringAsFixed(2) for consistent decimal display

Future Enhancements
Add temperature unit toggles

Implement history persistence

Add theme customization

Incorporate additional conversion units (Kelvin)

Add unit tests

Contribution Guidelines
Fork the repository

Create a feature branch (git checkout -b feature/improvement)

Commit changes (git commit -m 'Add some feature')

Push to branch (git push origin feature/improvement)

Open a pull request
