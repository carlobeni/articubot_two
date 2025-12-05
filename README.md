# I. Crear y Registrar un Workspace en ROS 2
## 1. Crear un nuevo workspace

```bash
mkdir -p ~/ros2_robot_ws3/src
cd ~/ros2_robot_ws3
```
## 2. Clonar repositorio en src
```bash
git clone https://github.com/carlobeni/articubot_two.git
```

## 3. Agregar twist_stamper y twist_mux
```bash
sudo apt update # actualizacion de dependencias
sudo apt install ros-rolling-twist-stamper
sudo apt install ros-rolling-twist-mux
```
OBS: Cambiar rolling por humble, iron, etc segun versión de ROS
## 4. Compilacion automatica del workspace con colcon

```bash
colcon build --symlink-install
```

## 5. Cargar el workspace en la terminal actual

```bash
source install/setup.bash
```

## 6. Registrar el workspace en el archivo `.bashrc` (Alternativo al paso 5 con efecto permanente)

Abrir el archivo:

```bash
code ~/.bashrc
```

Agregar al final del archivo:

```bash
source /opt/ros/$ROS_DISTRO/setup.bash
source ~/ros2_robot_ws3/install/setup.bash # verificar el path correcto
```


# 2. Simulacion con Gazebo
Terminal 1: Correr empty world
```bash
ros2 launch articubot_two launch_sim.launch.py
```

Terminal 2: Cargar world preguardado, ejemplo con obstacles.world
```bash
ros2 launch articubot_two launch_sim.launch.py world:=./src/articubot_two/worlds/obstacles.world
```
# 3.Lectura de topics en Gazebo
## 1. Ver lista de Topics de Gazebo
Agregar Topic Viewer
## 2. Lectura de Messages de /topic_name
1. Agregar Topic Echo
2. Cargar topic, ejemplo: /cmd_vel
3. Cambiar a modo pop-screen con el botor cuadrado

# 4. Simulacion con RViz (Opcional)
## 1. Abrir RViz
```bash
ros2 run rviz2 rviz2
``` 
## 2. Correr launch
```bash
ros2 launch articubot_two launch_sim.launch.py
```
## 3. Agregar las fuentes de RViz
1. En Global Options, agregar las fuentes de RViz
2. Agregar TF
3. Agregar Robot Model con Topic: /robot_description


# 5. ros2_control
## 1. Instalar ros2_control
```bash
sudo apt update
sudo apt install ros-rolling-ros2-control ros-rolling-ros2-controllers # para implementacion real
sudo apt install ros-rolling-gz-ros2-control # para simulacion
```
OBS: Cambiar rolling por humble, iron, etc segun versión de ROS

## 2. Veriifcar que los plugins de ros2_control esten activos
```bash
ros2 control list_controllers
```

## 3. Control con teleop_twist_keyboard con /cmd_vel:=/diff_cont/cmd_vel_unstamped
```bash
ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args -r /cmd_vel:=/diff_cont/cmd_vel_unstamped
```