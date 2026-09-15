# SBC-SlowControl
SBC-SlowControl

The active source files are located in the slowcontrol_reconstruct directory. The entire project — architecture, daemon, GUI, and alarm system — was independently designed and developed by the author.

The Scintillating Bubble Chamber (SBC) is a bubble-chamber detector for dark matter searches. Stable detection requires a stable thermodynamic environment, maintained by this slow control system, which manages 120+ instruments including temperature and pressure transducers, pneumatic and solenoid valves, heaters, and photon sensors.

The system runs on two main programs: SBC_background.py and SBC_GUI.py.

SBC_background.py is a daemon that continuously acquires data from the detector, detects and broadcasts alarms, and writes data to a MySQL database. Alarms are threshold-based and broadcast via Slack. MySQL was chosen for reliable long-term storage of time-series data and compatibility with the Grafana/SeeQ visualization tools.

These functions must run 24/7 to meet the requirement of uninterrupted data collection. To handle unexpected crashes and power outages, the program is launched by BKGcron_init.sh, invoked by the Linux crontab: BKGcron_init.sh restarts the program after a crash, while crontab relaunches it after a system reboot following power recovery. The background daemon has been running continuously at Fermilab for 4+ years.

SBC_background.py also contains an UpdateServer class that communicates with SBC_GUI.py, isolating the human interface from the continuously running data pipeline and preserving the stability of the daemon.

SBC_GUI.py provides a multi-tab graphical interface for manual control of instrument states (valves, heaters, alarms). It covers 120+ instruments and supports display customization, including expanding/collapsing widgets and switching between subsystems.

slowcontrol_reconstruct/
├── SBC_background.py       # daemon: data acquisition, alarms, DB writes
├── SBC_GUI.py               # operator interface
├── SBC_GUI_Widgets.py       # operator interface graphic design
├── SBC_env.py               # background environmental variables
├── SBC_alarm_autoload.py    # alarm pre-loading from local configuration files
├── SBC_watchdog_database.py  # alarm and MySQL protocol classes
├── BKGcron_init.sh          # crash/reboot recovery wrapper
└── utils/                   # auxiliary functions


