# Temperature Converter App

## Overview
The Temperature Converter is a responsive Flutter application that allows users to convert between Fahrenheit and Celsius temperature scales. The app features dual-orientation support (portrait and landscape), maintains a history of conversions, and provides precise calculations using standard conversion formulas.

## Features
- **Dual Conversion Modes**: Fahrenheit ↔ Celsius
- **Precise Calculations**: 2-decimal place accuracy
- **Conversion History**: Session-based history tracking
- **Responsive Design**: Optimized layouts for portrait/landscape
- **Intuitive UI**: Clean Material Design interface
- **Input Validation**: Handles invalid entries gracefully

## Technical Architecture

### Component Structure
```
lib/
└── main.dart
    ├── TemperatureConverterApp (Root widget)
    └── ConverterScreen (Main screen)
        ├── _ConverterScreenState (State class)
        │   ├── _convert() - Conversion logic
        │   ├── _buildConversionSelector()
        │   ├── _buildInputField()
        │   ├── _buildHistoryList()
        │   ├── _buildPortraitLayout()
        │   └── _buildLandscapeLayout()
        └── HistoryItem (Data model)
```

### Dependencies
- Flutter SDK (minimum 3.0.0)
- Material Design widgets (included in Flutter)

## Critical Components

### 1. Conversion Logic
```dart
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
```
- Validates numeric input
- Applies precise conversion formulas
- Maintains history with most recent first
- Updates UI with formatted result

### 2. Responsive Layout
```dart
OrientationBuilder(
  builder: (context, orientation) {
    return orientation == Orientation.portrait
        ? _buildPortraitLayout()
        : _buildLandscapeLayout();
  },
)
```
- **Portrait**: Vertical column layout
- **Landscape**: Horizontal split-screen layout
- Automatically adapts to device orientation

### 3. History Management
```dart
class HistoryItem {
  final ConversionType type;
  final double input;
  final double result;

  HistoryItem({required this.type, required this.input, required this.result});
}

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
```
- Structured data model for history items
- Efficient ListView rendering
- Most recent conversions at top

## Getting Started

### Prerequisites
- Flutter SDK (v3.0+)
- Dart SDK (v2.17+)
- Android Studio/Xcode (for emulators)

### Installation
1. Clone the repository:
   ```bash
   git clone https://github.com/olaniyiolufemi/temp-converter/
   ```
2. Install dependencies:
   ```bash
   flutter pub get
   ```
3. Run the app:
   ```bash
   flutter run
   ```

## Usage Guide
1. **Select conversion type** using radio buttons
2. **Enter temperature** in the input field
3. Press **Convert** to calculate
4. View **result** below the button
5. **History** updates automatically
6. **Rotate device** to switch layouts

## Testing
Manual test cases include:
1. Conversion accuracy:
   - 32°F → 0°C
   - 0°C → 32°F
   - 100°C → 212°F
   - 212°F → 100°C
2. History tracking
3. Orientation responsiveness
4. Input validation
5. UI consistency

## Technical Decisions
1. **State Management**: Uses `setState` for simplicity
2. **Responsive Design**: `OrientationBuilder` for layout switching
3. **Data Modeling**: Dedicated `HistoryItem` class
4. **Input Handling**: Numeric keyboard with validation
5. **Precision**: `toStringAsFixed(2)` for decimal formatting

## Future Enhancements
1. Add Kelvin conversion
2. Implement history persistence
3. Theme customization
4. Unit conversion toggles
5. Additional temperature scales

## Contributing
1. Fork the repository
2. Create your feature branch:
   ```bash
   git checkout -b feature/improvement
   ```
3. Commit your changes:
   ```bash
   git commit -m 'Add some feature'
   ```
4. Push to the branch:
   ```bash
   git push origin feature/improvement
   ```
5. Open a pull request
