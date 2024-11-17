# DReyeVR
### Welcome to DReyeVR, a VR driving simulator for behavioural and interactions research.
<!-- Welcome to the DReyeVR wiki! -->

This project extends the [`Carla`](https://github.com/carla-simulator/carla/tree/0.9.13) simulator to add virtual reality integration, a first-person maneuverable ego-vehicle, eye tracking support, and several immersion enhancements.

**IMPORTANT:** Currently DReyeVR only supports Carla versions: [0.9.13](https://github.com/carla-simulator/carla/tree/0.9.13) with Unreal Engine 4.26

## Highlights
### Ego Vehicle
Fully drivable **virtual reality (VR) ego-vehicle** with [SteamVR integration](https://github.com/ValveSoftware/steamvr_unreal_plugin/tree/4.23) (see [EgoVehicle.h](DReyeVR/EgoVehicle.h))
- SteamVR HMD head tracking (orientation & position)
  - We have tested with the following devices:
    | Device | VR Supported | Eye tracking | OS |
    | --- | --- | --- | --- |
    | [HTC Vive Pro Eye](https://business.vive.com/us/product/vive-pro-eye-office/) | :white_check_mark: | :white_check_mark: | Windows, Linux |
    | [Quest 2](https://www.oculus.com/quest-2/) | :white_check_mark: | :x: | Windows |
  - While we haven't tested other headsets, they should still work for basic VR usage (not eye tracking) if supported by SteamVR.
  - Eye tracking is currently **ONLY** supported on the HTC Vive Pro Eye since we use [SRanipal](https://forum.htc.com/topic/5641-sranipal-faq/) for the eye-tracker SDK. We are happy to support more devices through contributions for adding other SDKs. 
- Vehicle controls:
  - Generic keyboard WASD + mouse
  - Support for Logitech Steering wheel
    - Includes force-feedback with the steering wheel.
    - We used a [Logitech G923 Racing Wheel & Pedals](https://www.logitechg.com/en-us/products/driving/driving-force-racing-wheel.html)
- Realistic (and parameterizable) rear & side view mirrors 
  - WARNING: very performance intensive
- Vehicle dashboard:
  - Speedometer (in miles-per-hour by default)
  - Gear indicator
  - Turn signals
- Dynamic steering wheel
  - Adjustable parameters, responsive to steering input
  - See our documentation on this [here](Docs/Model.md)
- "Ego-centric" audio 
  - Responsive engine revving (throttle-based)
  - Turn signal clicks
  - Gear switching
  - Collisions
- Fully compatible with the existing Carla [PythonAPI](https://carla.readthedocs.io/en/0.9.13/python_api/) and [ScenarioRunner](https://github.com/carla-simulator/scenario_runner/tree/v0.9.13)
  - Minor modifications were made. See [Usage.md](Docs/Usage.md) documentation.
- Fully compatible with the Carla [Recorder and Replayer](https://carla.readthedocs.io/en/0.9.13/adv_recorder/) 
  - Including HMD pose/orientation & sensor reenactment 
- Ability to handoff/takeover control to/from Carla's AI wheeled vehicle controller
- Carla-based semantic segmentation camera (see [`Shaders/README.md`](Shaders/README.md))
### Ego Sensor
Carla-compatible **ego-vehicle sensor** (see [EgoSensor.h](DReyeVR/EgoSensor.h)) is an "invisible sensor" that tracks the following:
- Real-time **Eye tracking** with the [HTC Vive Pro Eye](https://enterprise.vive.com/us/product/vive-pro-eye-office/) VR headset
  - Eye tracker data includes:
    - Timing information (based off headset, world, and eye-tracker)
    - 3D Eye gaze ray (left, right, & combined)
    - 2D Pupil position (left & right)
    - Pupil diameter (left & right)
    - Eye Openness (left & right)
    - Focus point in the world & hit actor information
    - See [DReyeVRData.h:EyeTracker](Carla/Sensor/DReyeVRData.h) for the complete list
  - Eye reticle visualization in real time
- Real-time user inputs (throttle, steering, brake, turn signals, etc.)
- Image (screenshot) frame capture based on the camera 
  - Typically used in Replay rather than real-time because highly performance intensive.
- Fully compatible with the LibCarla data serialization for streaming to a PythonAPI client (see [LibCarla/Sensor](LibCarla/Sensor))
  - We have also tested and verified support for (`rospy`) ROS integration our sensor data streams

### Other additions:
- Custom DReyeVR config file for one-time runtime params. See [DReyeVRConfig.ini](Configs/DReyeVRConfig.ini)
  - Especially useful to change params without recompiling everything.
  - Uses standard c++ io management to read the file with minimal performance impact. See [DReyeVRUtils.h](DReyeVR/DReyeVRUtils.h).
- World ambient audio
  - Birdsong, wind, smoke, etc. (See [Docs/Sounds.md](Docs/Sounds.md))
- Non-ego-centric audio (Engine revving from non-ego vehicles)
- Synchronized Replay with per-frame frame capture for post-hoc analysis (See [Docs/Usage.md](Docs/Usage.md))
- Recorder/replayer media functions
  - Added in-game keyboard commands Play/Pause/Forward/Backward/etc.
- Static in-environment directional signs for natural navigation (See [`Docs/Signs.md`](Docs/Signs.md))
- Adding weather to the Carla recorder/replayer/query (See this [Carla PR](https://github.com/carla-simulator/carla/pull/5235))
- Custom dynamic 3D actors with full recording support (eg. HUD indicators for direction, AR bounding boxes, visual targets, etc.). See [CustomActor.md](Docs/CustomActor.md) for more.
- (DEBUG ONLY) Foveated rendering for improved performance with gaze-aware (or fixed) variable rate shading

## Install/Build
See [`Docs/Install.md`](Docs/Install.md) to:
- Install and build `DReyeVR` on top of a working `Carla` repository. 
- Download plugins for `DReyeVR` required for fancy features such as:
  - Eye tracking (SRanipal)
  - Steering wheel/pedals (Logitech)
- Set up a `conda` environment for DReyeVR PythonAPI



## Documentation & Guides
- See [`Install.md`](Docs/Install.md) to install and build DReyeVR
- See [`Usage.md`](Docs/Usage.md) to learn how to use our provided DReyeVR features
- See [`Development.md`](Docs/Development.md) to get started with DReyeVR development and add new features
- See [`Docs/Tutorials/`](Docs/Tutorials/) to view several DReyeVR tutorials such as customizing the EgoVehicle, adding custom signs/props and more.



## Reference
- [Paper](https://arxiv.org/abs/2201.01931)
