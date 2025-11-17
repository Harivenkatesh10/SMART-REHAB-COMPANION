# Smart Rehabilitation Companion

An affordable, wearable rehabilitation system for consistent physiotherapy monitoring with real-time feedback and remote IoT integration.

## 🎯 Project Overview

The **Smart Rehabilitation Companion** is a hardware-based rehabilitation device designed to address the challenges patients face during recovery from neurological and orthopedic conditions. By integrating **EMG muscle monitoring**, **motion tracking (IMU)**, and **grip strength analysis**, this system provides **real-time feedback** and enables **remote physiotherapist supervision** through an IoT-enabled mobile app.

### Key Problem Solved
> "Patients struggle with therapy consistency & progress tracking"

Traditional rehabilitation relies on frequent in-person physiotherapist visits, which are often:
- Time-consuming and costly for patients
- Labor-intensive for healthcare providers
- Inconsistent in therapy adherence and exercise execution
- Difficult to track long-term progress

This system enables **autonomous home-based rehabilitation** with professional oversight, improving therapy consistency and patient outcomes.

---

## ✨ Features

### Real-Time Monitoring
- **EMG Muscle Activation Detection**: Ensures patients engage the correct muscles during exercises
- **IMU Motion Tracking**: Validates wrist flexion/extension, pronation/supination with ±3° accuracy
- **Grip Strength Analysis**: Measures and categorizes grip force levels in real-time

### Intelligent Feedback System
- **LCD Display Guidance**: Shows real-time feedback messages ("Doing Good", "Muscle Not Activated - Try Again", "Take Rest")
- **LED Indicators**: Change colors based on grip force levels
- **Buzzer Alerts**: Vibrate notifications for incorrect movements or excessive fatigue

### Remote Monitoring & IoT Integration
- **Blynk Cloud Platform**: Physiotherapists can monitor patient progress remotely
- **Wi-Fi Connectivity**: Real-time data transmission for cloud storage and analysis
- **Progress Tracking**: Automated logging of muscle activation, movement patterns, and grip strength

### Hardware Optimization
- **Battery Powered**: 3+ hours of continuous operation per charge
- **Wearable Design**: Lightweight forearm band for comfort during therapy
- **Interactive Smart Ball**: Adaptive pressure resistance for varied grip strength training

---

## 🔧 System Architecture

### Four Core Components

#### 1. **Wearable Band** (Forearm-Worn Sensor Module)
- **AD8232 EMG Sensor**: Detects forearm muscle activation
- **MPU6050 IMU**: Tracks wrist flexion/extension and rotational movements
- **FSR402 Force Sensor**: Measures initial grip pressure feedback
- **Electrode Placement**: On flexor digitorum superficialis and pronator teres muscles

#### 2. **Interactive Smart Ball** (Grip Strength Device)
- **FSR402/FSR406 Force Sensor**: Measures grip strength applied to the ball
- **Honeywell MPRLS0025PA000 Pressure Sensor**: Dynamically adjusts internal ball pressure
- **Adaptive Resistance**: Changes resistance based on therapy needs and patient performance

#### 3. **Master Control Board** (Central Processing Unit)
- **Microcontroller**: Raspberry Pi Pico W
- **Real-Time Data Processing**: Processes EMG, IMU, and force sensor inputs
- **Feedback Control**: Manages LCD, LED, and buzzer outputs
- **Communication Module**: Wi-Fi & Bluetooth for Blynk IoT app integration
- **Power Management**: TP4056 Li-Ion battery with BMS protection

#### 4. **Mobile App** (Remote Monitoring Interface)
- **Blynk IoT Platform**: Physiotherapist dashboard
- **Real-Time Metrics**: EMG values, grip strength, movement angles
- **Patient Progress Tracking**: Historical data and trend analysis
- **Remote Supervision**: Allows therapists to adjust therapy recommendations

---

## 📋 Hardware Requirements

| Component | Model | Pin/Interface | Purpose |
|-----------|-------|----------------|---------|
| Microcontroller | Raspberry Pi Pico W | Central | Main processor & communication hub |
| EMG Sensor | AD8232 | GP26/ADC0 | Muscle activation detection |
| IMU Sensor | MPU6050 | I2C (GP0/GP1) | Motion & rotation tracking |
| Force Sensors | FSR402 | GP28/ADC2 | Grip strength measurement |
| Pressure Sensor | Honeywell MPRLS0025PA000 | I2C | Ball pressure regulation |
| Motor Driver | DRV8833 / L298N | GPIO | Pressure adjustment control |
| Feedback System | LED + Buzzer + LCD | GPIO | Real-time user alerts |
| LCD Display | I2C 16x2 LCD | I2C | Exercise guidance messages |
| Battery | TP4056 Li-Ion (2000mAh) | USB/JST | Power supply with BMS |
| Communication | Built-in (Pico W) | Wi-Fi/BLE | Blynk IoT connectivity |

---

## 💻 Software Stack

- **Language**: MicroPython
- **Microcontroller Platform**: Raspberry Pi Pico W
- **IoT Backend**: Blynk Cloud Platform
- **Communication Protocols**: Wi-Fi, Bluetooth LE, I2C, UART
- **Development Environment**: Thonny IDE / VSCode

---

## 📊 Performance Metrics

| Metric | Result | Status |
|--------|--------|--------|
| EMG Detection Accuracy | 92% | ✅ Excellent |
| IMU Angular Accuracy | ±3° | ✅ Good |
| Grip Force Response Time | 0.2s | ✅ Real-time |
| Battery Life (Continuous) | 3+ hours | ✅ Acceptable |
| Wi-Fi Data Sync Latency | <500ms | ✅ Low |

---

## 🚀 Installation & Setup

### Prerequisites
1. Raspberry Pi Pico W microcontroller
2. MicroPython firmware flashed on Pico W
3. Thonny IDE or similar code editor
4. Blynk app installed on smartphone (Android/iOS)
5. All sensors and components (refer to Hardware Requirements table)

### Step-by-Step Setup

#### 1. Flash MicroPython Firmware
```bash
# Download latest MicroPython firmware for Pico W from micropython.org
# Use Thonny IDE: Tools > Options > Interpreter > Select Pico W and install firmware
```

#### 2. Wire the Hardware
Connect sensors to Pico W according to the circuit diagram:
- **I2C (IMU/LCD)**: GP0 (SDA), GP1 (SCL)
- **ADC (Force Sensor)**: GP28
- **ADC (EMG Sensor)**: GP26
- **GPIO (LED)**: GP15
- **GPIO (Buzzer)**: GP14 (recommended)

#### 3. Upload Firmware Code
1. Open `firmware/main.py` in Thonny
2. Update WiFi credentials in `config.py`:
```python
WIFI_SSID = "Your_WiFi_Name"
WIFI_PASSWORD = "Your_WiFi_Password"
BLYNK_AUTH = "Your_Blynk_Auth_Token"
```
3. Upload files to Pico W

#### 4. Configure Blynk App
1. Create a Blynk account at blynk.io
2. Create new template for rehabilitation data
3. Add virtual pins:
   - V0: EMG Value
   - V1: Grip Strength
   - V2-V4: Gyroscope (X, Y, Z)
   - V5: Accelerometer (X)
4. Copy auth token to `config.py`

#### 5. Sensor Calibration
Run calibration routines before first use:
```bash
python tests/sensor_calibration.py
python tests/performance_testing.py
```

---

## 📁 Project Structure

```
smart-rehab-companion/
├── README.md                              # Main project overview
├── LICENSE                                # MIT License
├── .gitignore                             # Git ignore rules
│
├── firmware/                              # MicroPython code for Pico W
│   ├── main.py                            # Main firmware code
│   ├── imu.py                             # MPU6050 library
│   ├── config.py                          # Configuration (credentials & thresholds)
│   └── blynk_lib.py                       # Blynk IoT library
│
├── hardware/                              # Circuit designs & component files
│   ├── BOM.md                             # Bill of Materials with pricing
│   ├── circuit_diagram.pdf                # Electrical schematic
│   ├── PCB_design/                        # PCB layout files (if available)
│   └── assembly_guide.md                  # Hardware assembly instructions
│
├── documentation/                         # Technical documentation
│   ├── INSTALLATION.md                    # Detailed setup guide
│   ├── SENSOR_CALIBRATION.md              # Sensor calibration procedures
│   ├── TECHNICAL_SPECIFICATIONS.md        # System specs & performance
│   ├── TROUBLESHOOTING.md                 # Common issues & solutions
│   └── API_REFERENCE.md                   # Firmware API documentation
│
├── docs/                                  # Visual documentation
│   ├── block_diagram.png                  # System block diagram
│   ├── architecture_diagram.png           # Architecture overview
│   ├── circuit_diagram.png                # Circuit schematic image
│   └── flow_diagram.png                   # Data flow diagram
│
└── tests/                                 # Testing & validation scripts
    ├── sensor_calibration.py              # EMG, IMU, Force sensor calibration
    ├── performance_testing.py             # Accuracy & latency testing
    └── sample_data.csv                    # Example sensor readings
```

---

## 🔄 Data Flow & Operation

### Exercise Session Workflow

1. **Initialization Phase**
   - System powers on and connects to Wi-Fi
   - Sensors initialize and calibrate
   - Patient launches Blynk app on phone

2. **Real-Time Monitoring Phase**
   - EMG sensor detects muscle activation
   - IMU tracks wrist movement patterns
   - Force sensor measures grip strength
   - Master board processes all inputs simultaneously

3. **Feedback Generation Phase**
   - System evaluates exercise correctness:
     - ✅ **Correct**: LCD shows "Doing Good!", LED turns green
     - ❌ **Incorrect**: LCD shows "Muscle Not Activated - Try Again", Buzzer alerts
     - ⚠️ **Overexertion**: LCD shows "Take Rest!", LED turns red

4. **Remote Monitoring Phase**
   - Data transmitted to Blynk cloud in real-time
   - Physiotherapist reviews metrics on mobile dashboard
   - Historical data stored for progress tracking

5. **Session End**
   - System logs session statistics
   - Patient data synced to cloud
   - Battery returns to low-power standby mode

---

## 📈 Performance Results

### EMG Sensor Accuracy
- **Detection Rate**: 92% accuracy in identifying correct muscle activation
- **False Positives**: Minimal when electrodes properly placed
- **Response Time**: <100ms
- **Challenge**: Electrode placement precision critical for consistent results

### IMU Motion Tracking
- **Angular Accuracy**: ±3° in controlled conditions
- **Drift Issues**: Minor accumulation over extended sessions
- **Improvement Needed**: Sensor fusion with Kalman filtering for long sessions

### Grip Strength Monitoring
- **Force Categories**: 
  - Light Grip: 0-150 units
  - Medium Grip: 150-350 units
  - Strong Grip: 350-650 units
  - Maximum (Alert): >700 units
- **Response Time**: 0.2s per reading
- **Limitation**: Single-point measurement affects 3D pressure distribution

### Power Consumption
- **Continuous Operation**: 3+ hours per 2000mAh charge
- **Peak Draw**: During Wi-Fi data transmission
- **Idle Current**: ~50mA (main system) + 20mA (sensors)
- **Optimization Achieved**: LCD brightness reduction → 15% battery life improvement

---

## 🛠️ Key Challenges & Solutions

| Challenge | Root Cause | Solution Implemented |
|-----------|-----------|----------------------|
| EMG Signal Noise | Electrical interference & muscle tremors | Adaptive filtering, electrode calibration |
| IMU Drift | Gyroscope integration errors | Sensor fusion approach planned |
| Grip Sensing Limitations | Single-point force measurement | Multi-sensor array planned for v2 |
| Battery Drain | Continuous Wi-Fi transmission | Dynamic power-saving algorithms |
| Data Synchronization | Network latency & packet loss | Blynk protocol with retry logic |

---

## 🔮 Future Enhancements

### Planned Improvements (v2.0)

**AI-Driven Adaptation**
- Machine learning models for personalized therapy recommendations
- Adaptive exercise difficulty based on patient performance trends
- Predictive fatigue detection

**Enhanced Sensing**
- Multi-point force sensor array for 3D grip mapping
- Multiple IMU units for full-body motion tracking
- Pressure distribution analysis

**Extended Battery Life**
- Dynamic power-saving: 4-5 hours operation
- Optimized Bluetooth Low Energy (BLE) for reduced power consumption
- Solar charging integration for prolonged use

**Gamified Rehabilitation**
- Rehabilitation game interface with achievement badges
- Competitive leaderboards for patient motivation
- Progress visualization dashboards

**Clinical Integration**
- Export compatibility with medical records systems (HL7/FHIR)
- Integration with physical therapy management software
- Advanced analytics for therapist decision-making

**Multi-Body Support**
- Expansion beyond arm/wrist to include lower limb rehabilitation
- Modular sensor design for targeted therapy
- Combination protocols for full-body recovery

---

## 📝 Usage Examples

### For Patients
1. Wear the forearm band with proper electrode placement
2. Hold the interactive smart ball
3. Follow LCD guidance for exercise instructions
4. Perform repetitive exercises as guided
5. View real-time feedback on LED/buzzer indicators

### For Physiotherapists
1. Monitor multiple patients via Blynk dashboard
2. Review real-time EMG, motion, and grip strength metrics
3. Adjust exercise recommendations based on performance
4. Track long-term recovery progress through data trends
5. Generate rehabilitation reports for clinical documentation

---

## 🧪 Testing & Validation

### Recommended Testing Protocol
```bash
# Calibrate all sensors before deployment
python tests/sensor_calibration.py

# Run performance tests
python tests/performance_testing.py

# Validate Blynk connectivity
# Launch main.py and monitor console for successful Wi-Fi/Blynk connection
```

### Test Scenarios
1. **EMG Accuracy**: Verify correct muscle activation detection
2. **Motion Tracking**: Test wrist movement angle accuracy within ±3°
3. **Force Measurement**: Validate grip categories (light/medium/strong)
4. **Battery Life**: Log continuous operation duration
5. **Cloud Synchronization**: Check real-time data transmission to Blynk

---

## 🤝 Contributing

Contributions are welcome! To contribute:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement-name`)
3. Make your changes and test thoroughly
4. Commit with descriptive messages (`git commit -m "Add feature: description"`)
5. Push to your branch (`git push origin feature/improvement-name`)
6. Open a Pull Request with detailed description

### Contribution Areas
- Sensor calibration improvements
- Algorithm optimization (EMG filtering, Kalman filtering)
- Documentation enhancements
- New feedback mechanisms
- UI/UX improvements for Blynk app

---

## 🐛 Troubleshooting

### Common Issues

**No EMG Signal Detected**
- Check electrode placement on forearm muscles
- Verify electrode-to-skin contact
- Run EMG calibration routine
- Check ADC pin connection (GP26)

**IMU Not Responding**
- Verify I2C connection (GP0/GP1)
- Check I2C clock frequency (should be 400kHz)
- Confirm MPU6050 address (0x68)
- Test with i2c_scanner.py to verify communication

**Blynk Connection Failed**
- Verify Wi-Fi SSID and password in config.py
- Check router connectivity and signal strength
- Verify Blynk auth token is correct
- Review Blynk app authentication settings

**Low Battery Life**
- Reduce LCD brightness
- Minimize buzzer activation duration
- Enable deep sleep mode
- Consider higher capacity battery (2200mAh+)

**Inconsistent Grip Readings**
- Ensure force sensor is properly calibrated
- Check sensor placement inside ball
- Verify FSR402 sensor is not damaged
- Run force sensor calibration test

For detailed troubleshooting, see [TROUBLESHOOTING.md](documentation/TROUBLESHOOTING.md)

---

## 📚 Additional Resources

- [Blynk Documentation](https://docs.blynk.io/)
- [Raspberry Pi Pico W Documentation](https://www.raspberrypi.com/documentation/microcontrollers/)
- [MicroPython Documentation](https://docs.micropython.org/)
- [AD8232 EMG Sensor Guide](https://learn.sparkfun.com/tutorials/ad8232-heart-rate-monitor-sensor-hookup-guide)
- [MPU6050 IMU Guide](https://learn.sparkfun.com/tutorials/mpu-6050-accelerometer-and-gyro-sensor-tutorial)

---

## 📄 License

This project is licensed under the **MIT License** - see [LICENSE](LICENSE) file for details.

This means you are free to use, modify, and distribute this project for academic and commercial purposes with proper attribution.

---

## 👥 Authors & Affiliation

**Developed at**: Chennai Institute of Technology  
**Department**: Electronics & Communication Engineering  
**Project Type**: Hardware-Software Co-design Capstone Project

---

## 📞 Support & Feedback

- **Issues**: Report bugs via GitHub Issues
- **Questions**: Check existing discussions or create new ones
- **Feedback**: Suggestions for improvement are welcome!

---

## 🙏 Acknowledgments

- Physiotherapy domain experts for clinical guidance
- Chennai Institute of Technology for research support
- Open-source communities for MicroPython and Blynk platforms
- All contributors and testers who validated the prototype

---

**Last Updated**: November 17, 2025  
**Version**: 1.0 (Initial Release)

---

*Transform rehabilitation therapy with intelligent, accessible, and affordable technology.*
