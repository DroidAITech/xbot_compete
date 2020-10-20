# 安徽省机器人大赛-服务机器人比赛demo

## 简介

该机器人比赛ROS程序包包含了比赛所需要的配置文件、地图以及相应的启动程序。

以2019年比赛场地建立的地图为标准，参赛队在此地图上实现机器人的导航。

改程序包需配合xbot机器人的驱动程序包使用，驱动程序包下载地址为：

https://github.com/DroidAITech/xbot/archive/kinetic-devel.zip

请使用kinetic-devel分支的程序。

xbot机器人的驱动程序包的ROS接口说明见：

https://www.droid.ac.cn/docs/products/xbotu/userguide/3.2.3/roswiki.html

## 如何使用？

### 编写代码

参赛队参考机器人的ROS接口，按照比赛规则的要求，完成该程序包中的script/demo文件夹下的程序。

在程序中，demo已经给出了大部分比赛程序的框架和结构。

参赛队需要修改代码文件demo.py中注释了参赛队需要完成的部分。

```python
#!/usr/bin/env python
#coding=utf-8

import rospy, yaml, os, json,time

from xbot_face.msg import FaceResult
from xbot_talker.srv import chat, play
from std_msgs.msg import String, UInt32, UInt8, Bool
from geometry_msgs.msg import Pose, PoseStamped
from actionlib_msgs.msg import GoalStatusArray
from move_base_msgs.msg import MoveBaseActionResult
from std_srvs.srv import Empty

class compete():
	"""docstring for welcome"""
	def __init__(self):
#       声明节点订阅与发布的消息

		# 发布目标点信息
		self.move_base_goal_pub = rospy.Publisher('/move_base_simple/goal', PoseStamped, queue_size=1)
		# 订阅人脸识别结果
		self.face_result_sub = rospy.Subscriber('/xbot/face_result', FaceResult, self.faceCB)
		# 订阅是否到达目标点结果
		self.move_base_result_sub = rospy.Subscriber('/move_base/result', MoveBaseActionResult, self.move_base_resultCB)
		# 请求清除costmap服务
		self.clear_costmaps_srv = rospy.ServiceProxy('/move_base/clear_costmaps',Empty)
		# 订阅是否收到visit信息
		self.visit_sub = rospy.Subscriber('/demo/leave', Bool, self.visitCB)

		# 请求chat服务
		self.chat_srv = rospy.ServiceProxy('/xbot/chat',chat)
		# 请求播放服务
		self.play_srv = rospy.ServiceProxy('/xbot/play',play)


		#        读取一存储的讲解点字典文件,默认位于xbot_s/param/position_dic.yaml文件
		self.kp_path = rospy.get_param('/compete/kp_path','/home/xbot/catkin_ws/src/xbot_compete/script/demo/kp.json')
		self.greet_path = rospy.get_param('/compete/greet_path','/home/xbot/catkin_ws/src/xbot_compete/script/demo/greet.json')
#		yaml_path = yaml_path + '/scripts/position_dic.yaml'
		with open(self.kp_path, 'r') as json_file:
			self.kp_list = json.load(json_file)
		json_file.close()

		with open(self.greet_path,'r') as f:
			self.greet_dict = json.load(f)
		f.close()
		rospy.spin()




	def greeting(self,name):
		# 参赛队需要完善的代码
		pass



	def faceCB(self,msg):
		# 参赛队需要完善的代码
		pass

	def pub_kp(self):
		# 参赛队需要完善的代码
		pass


	def visitCB(self, msg):
		# 参赛队需要完善的代码
		pass


#    导航程序对前往目标点的执行结果
	def move_base_resultCB(self, result):
		# 参赛队需要完善的代码
		pass

if __name__ == '__main__':
	rospy.init_node('compete_node')
	try:
		rospy.loginfo('compete node initialized...')
		compete()
	except rospy.ROSInterruptException:
		rospy.loginfo('compete node initialize failed, please retry...')

```

### 修改配置文件

script/demo目录下的greet.json与kp.json文件为配置文件，程序会读取该配置文件完成任务。

参赛队需要修改相应的配置文件，比赛讲解词使用《比赛用词.docx》文件中所给出的文字。

请务必注意修改好讲解词中本队队名的文字，否则得不到相应的分数。

