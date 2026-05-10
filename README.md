cd ~/ros2_ws/src

ros2 pkg create --build-type ament_python temp_pkg

cd temp_pkg/temp_pkg

nano temp_publisher.py

    import rclpy
    import random
  
    from rclpy.node import Node

    from std_msgs.msg import Float64


    class TempPublisher(Node):

    def __init__(self):

        super().__init__('temp_publisher')

        self.pub = self.create_publisher(
            Float64,
            '/temperature',
            10
        )

        self.create_timer(
            1.0,
            self.publish
        )

    def publish(self):

        msg = Float64()

        msg.data = 20.0 + random.uniform(-2.0, 2.0)

        self.pub.publish(msg)

        self.get_logger().info(
            f'Temp: {msg.data:.2f}°C'
        )


    def main(args=None):

    rclpy.init(args=args)

    node = TempPublisher()

    rclpy.spin(node)

    rclpy.shutdown()

---------------------------

chmod +x temp_publisher.py

cd ~/ros2_ws/src/temp_pkg


    ----remplace dans:
      
    entry_points={

    'console_scripts': [
    
----avec:

    entry_points={
   
    'console_scripts': [
        'temp_publisher = temp_pkg.temp_publisher:main',
    ],
    },
-----------------------

nano package.xml

----ajoute avant test depend:

`<depend>rclpy</depend>`

`<depend>std_msgs</depend>`

-------------------

cd ~/ros2_ws

colcon build

source install/setup.bash

ros2 run temp_pkg temp_publisher

[INFO] [1778434444.060132171] [temp_publisher]: Temp: 18.98°C

[INFO] [1778434445.040772734] [temp_publisher]: Temp: 18.86°C

[INFO] [1778434446.043702639] [temp_publisher]: Temp: 20.93°C

[INFO] [1778434447.104714032] [temp_publisher]: Temp: 18.95°C

[INFO] [1778434448.043405147] [temp_publisher]: Temp: 20.96°C

[INFO] [1778434449.103702271] [temp_publisher]: Temp: 18.83°C

----dans un 2eme terminal:

ros2 topic echo /temperature

data: 21.167258855941167
---
data: 19.979778714370084
---
data: 21.29497948126432
---
data: 19.262989637623168
---
data: 21.581867091750304
---
data: 19.15869763896119
---
data: 21.38859298663388





if __name__ == '__main__':
    main()
