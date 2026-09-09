# AgileX camera assembly

This robot does not own capture or encode. Autostart names Scout topics
and ports. Shared backends:

| Role | Shared package | Scout launch |
| --- | --- | --- |
| ROS algorithms + RTP | `xgc2_camera_driver` V4L2 color then `xgc_ros_image_rtp` | `agilex_onboard_autostart/camera-media.launch` |
| ROS Image only | `xgc2_camera_driver` V4L2 color | `agilex_onboard_autostart/camera.launch` |
| WebRTC direct capture | `xgc2_camera_driver` `xgc_native_v4l2_rtp` | `agilex_onboard_autostart/webrtc.launch` |
| WebRTC while another process owns USB | `xgc2_camera_driver` `xgc_ros_image_rtp` | `agilex_onboard_autostart/fallback_webrtc.launch` |
| Browser fan-out | APT `xgc2-media-edge` | unit `xgc2-agilex-media-edge.service` |
| RViz | — | `agilex_onboard_rviz/rviz.launch` |

One physical sensor, one capture owner. `xgc2-agilex-camera` starts
`camera-media.launch` (`/d435/image_raw` plus RTP `:5004`). Stop that unit
before `webrtc.launch`. Media Edge does not open the camera. Roster is color
only. Public ICE address comes from `wlan1` unless `MEDIA_PUBLIC_IP` is set.
Do not revive `xgc2_camera_d435`.
