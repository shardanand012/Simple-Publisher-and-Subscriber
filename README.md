# Simple-Publisher-and-Subscriber

#  ROS 2 Python Project – Square a Number

This is a simple and beginner-friendly ROS 2 Python project that shows how to use a **publisher** and **subscriber** to send numbers, square them, and print the result using ROS 2 topics.

---

## Package Name

`number_square`

This is the ROS 2 package you will create. It contains two Python nodes:
1. A node to **publish numbers**
2. A node to **square the numbers and print the result**

---

##  What This Project Does

- One node sends numbers like 1, 2, 3...
- Another node receives each number, squares it (e.g., 3 → 9), and prints the result.
- This is done using **ROS 2 topics** and **Python code**.

---



---

##  How to Create This Project (Step-by-Step)

###  Step 1: Create the Package

Open terminal and run:
cd ~/ros2_ws/src
ros2 pkg create number_square --build-type ament_python --dependencies rclpy std_msgs

Add Your Python Scripts
Inside your package, go inside the terminal
cd ~/ros2_ws/src/number_square/number_square/

number_publisher.py
`This node publishes increasing numbers every second`
import rclpy
from rclpy.node import Node
from std_msgs.msg import Int32

class NumberPublisher(Node):
    def __init__(self):
        super().__init__('number_publisher')
        self.publisher_ = self.create_publisher(Int32, 'input_number', 10)
        self.timer = self.create_timer(1.0, self.publish_number)
        self.number = 1

    def publish_number(self):
        msg = Int32()
        msg.data = self.number
        self.publisher_.publish(msg)
        self.get_logger().info(f'Published: {self.number}')
        self.number += 1

def main(args=None):
    rclpy.init(args=args)
    node = NumberPublisher()
    rclpy.spin(node)
    node.destroy_node()
    rclpy.shutdown()
if_name_ == '_main_':
        main

square_and_print.py
`This node receives the number, squares it, and prints the result`
import rclpy
from rclpy.node import Node
from std_msgs.msg import Int32

class SquareAndPrintNode(Node):
    def __init__(self):
        super().__init__('square_and_print_node')
        self.subscription = self.create_subscription(
            Int32,
            'input_number',
            self.listener_callback,
            10
        )

    def listener_callback(self, msg):
        squared = msg.data ** 2
        self.get_logger().info(f'Input: {msg.data}, Squared: {squared}')

def main(args=None):
    rclpy.init(args=args)
    node = SquareAndPrintNode()
    rclpy.spin(node)
    node.destroy_node()
    rclpy.shutdown()
if_name_ == '_main_':
        main

Update setup.py
`Go to the main folder inside the terminal`
cd ~/ros2_ws/src/number_square/

Edit the setup.py file and make sure it looks like this
from setuptools import setup

package_name = 'number_square'

setup(
    name=package_name,
    version='0.0.1',
    packages=[package_name],
    data_files=[
        ('share/ament_index/resource_index/packages', ['resource/' + package_name]),
        ('share/' + package_name, ['package.xml', 'README.md']),
    ],
    install_requires=['setuptools'],
    zip_safe=True,
    maintainer='Your Name',
    maintainer_email='your@email.com',
    description='A simple ROS 2 Python project to square numbers using publishers and subscribers.',
    license='MIT',
    entry_points={
        'console_scripts': [
            'number_publisher = number_square.number_publisher:main',
            'square_and_print = number_square.square_and_print:main',
        ],
    },
)
