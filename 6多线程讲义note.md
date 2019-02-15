# 环境
- xubuntu 16.04
- anaconda
- pycharm
- python3.6
- https://www.cnblogs.com/jokerbj/p/7460260.html
- http://www.dabeaz.com/python/UnderstandingGIL.pdf

# 多线程 vs 多进程
- 程序：一堆代码以文本形式存入一个文档
- 进程： 程序运行的一个状态
    - 包含地址空间，内存，数据栈等
    - 每个进程由自己完全独立的运行环境，多进程共享数据是一个问题
- 线程
    - 一个进程的独立运行片段，一个进程可以由多个线程
    - 轻量化的进程
    - 一个进程的多个现成间共享数据和上下文运行环境
    - 共享互斥问题
- 全局解释器锁（GIL）
    - Python代码的执行是由python虚拟机进行控制
    - 在主循环中稚嫩更有一个控制线程在执行
  
- Python包
    - thread：有问题，不好用，python3改成了_thread
    -  threading: 通行的包
    - 案例01: 顺序执行，耗时比较长
			'''
			利用time函数，生成两个函数
			顺序调用
			计算总的运行时间

			'''
			import time

			def loop1():
				# ctime 得到当前时间
				print('Start loop 1 at :', time.ctime())
				# 睡眠多长时间，单位是秒
				time.sleep(4)
				print('End loop 1 at:', time.ctime())


			def loop2():
				# ctime 得到当前时间
				print('Start loop 2 at :', time.ctime())
				# 睡眠多长时间，单位是秒
				time.sleep(2)
				print('End loop 2 at:', time.ctime())


			def main():
				print("Starting at:", time.ctime())
				loop1()
				loop2()
				print("All done at:", time.ctime())


			if __name__ == '__main__':
				main()


    - 案例02： 改用多线程，缩短总时间，使用_thread
			'''
			利用time函数，生成两个函数
			顺序调用
			计算总的运行时间

			'''
			import time
			import _thread as thread


			def loop1():
				# ctime 得到当前时间
				print('Start loop 1 at :', time.ctime())
				# 睡眠多长时间，单位是秒
				time.sleep(4)
				print('End loop 1 at:', time.ctime())


			def loop2():
				# ctime 得到当前时间
				print('Start loop 2 at :', time.ctime())
				# 睡眠多长时间，单位是秒
				time.sleep(2)
				print('End loop 2 at:', time.ctime())


			def main():
				print("Starting at:", time.ctime())
				# 启动多线程的意思是用多线程去执行某个函数
				# 启动多线程函数为start_new_thead
				# 参数两个，一个是需要运行的函数名，第二是函数的参数作为元祖使用，为空则使用空元祖
				# 注意：如果函数只有一个参数，需要参数后由一个逗号
				thread.start_new_thread(loop1, ())

				thread.start_new_thread(loop2, ())

				print("All done at:", time.ctime())

			if __name__ == '__main__':
				main()
				while True:
					time.sleep(1)
    - 案例03： 多线程，传参数
			#利用time延时函数，生成两个函数
			# 利用多线程调用
			# 计算总运行时间
			# 练习带参数的多线程启动方法
			import time
			# 导入多线程包并更名为thread
			import _thread as thread

			def loop1(in1):
				# ctime 得到当前时间
				print('Start loop 1 at :', time.ctime())
				# 把参数打印出来
				print("我是参数 ",in1)
				# 睡眠多长时间，单位是秒
				time.sleep(4)
				print('End loop 1 at:', time.ctime())

			def loop2(in1, in2):
				# ctime 得到当前时间
				print('Start loop 2 at :', time.ctime())
				# 把参数in 和 in2打印出来，代表使用
				print("我是参数 " ,in1 , "和参数  ", in2)
				# 睡眠多长时间，单位是秒
				time.sleep(2)
				print('End loop 2 at:', time.ctime())



			def main():
				print("Starting at:", time.ctime())
				# 启动多线程的意思是用多线程去执行某个函数
				# 启动多线程函数为start_new_thead
				# 参数两个，一个是需要运行的函数名，第二是函数的参数作为元祖使用，为空则使用空元祖
				# 注意：如果函数只有一个参数，需要参数后由一个逗号
				thread.start_new_thread(loop1,("王老大", ))

				thread.start_new_thread(loop2,("王大鹏", "王晓鹏"))

				print("All done at:", time.ctime())

			if __name__ == "__main__":
				main()
				# 一定要有while语句
				# 因为启动多线程后本程序就作为主线程存在
				# 如果主线程执行完毕，则子线程可能也需要终止
				while True:
					time.sleep(10)
    
- threading的使用
    - 直接利用threading.Thread生成Thread实例
        1. t = threading.Thread(target=xxx, args=(xxx,))
        2. t.start():启动多线程
        3. t.join(): 等待多线程执行完成
        4. 案例04
			#利用time延时函数，生成两个函数
			# 利用多线程调用
			# 计算总运行时间
			# 练习带参数的多线程启动方法
			import time
			# 导入多线程处理包
			import threading

			def loop1(in1):
				# ctime 得到当前时间
				print('Start loop 1 at :', time.ctime())
				# 把参数打印出来
				print("我是参数 ",in1)
				# 睡眠多长时间，单位是秒
				time.sleep(4)
				print('End loop 1 at:', time.ctime())

			def loop2(in1, in2):
				# ctime 得到当前时间
				print('Start loop 2 at :', time.ctime())
				# 把参数in 和 in2打印出来，代表使用
				print("我是参数 " ,in1 , "和参数  ", in2)
				# 睡眠多长时间，单位是秒
				time.sleep(2)
				print('End loop 2 at:', time.ctime())


			def main():
				print("Starting at:", time.ctime())
				# 生成threading.Thread实例
				t1 = threading.Thread(target=loop1, args=("王老大",))
				t1.start()

				t2 = threading.Thread(target=loop2, args=("王大鹏", "王小鹏"))
				t2.start()

				print("All done at:", time.ctime())


			if __name__ == "__main__":
				main()
				# 一定要有while语句
				# 因为启动多线程后本程序就作为主线程存在
				# 如果主线程执行完毕，则子线程可能也需要终止
				while True:
					time.sleep(10)
        5. 案例05: 加入join后比较跟案例04的结果的异同
			#利用time延时函数，生成两个函数
			# 利用多线程调用
			# 计算总运行时间
			# 练习带参数的多线程启动方法
			import time
			# 导入多线程处理包
			import threading

			def loop1(in1):
				# ctime 得到当前时间
				print('Start loop 1 at :', time.ctime())
				# 把参数打印出来
				print("我是参数 ",in1)
				# 睡眠多长时间，单位是秒
				time.sleep(4)
				print('End loop 1 at:', time.ctime())

			def loop2(in1, in2):
				# ctime 得到当前时间
				print('Start loop 2 at :', time.ctime())
				# 把参数in 和 in2打印出来，代表使用
				print("我是参数 " ,in1 , "和参数  ", in2)
				# 睡眠多长时间，单位是秒
				time.sleep(2)
				print('End loop 2 at:', time.ctime())


			def main():
				print("Starting at:", time.ctime())
				# 生成threading.Thread实例
				t1 = threading.Thread(target=loop1, args=("王老大",))
				t1.start()

				t2 = threading.Thread(target=loop2, args=("王大鹏", "王小鹏"))
				t2.start()

				t1.join()
				t2.join()

				print("All done at:", time.ctime())


			if __name__ == "__main__":
				main()
				# 一定要有while语句
				# 因为启动多线程后本程序就作为主线程存在
				# 如果主线程执行完毕，则子线程可能也需要终止
				while True:
					time.sleep(10)

        - 守护线程-daemon
            - 如果在程序中将子线程设置成守护现成，则子线程会在主线程结束的时候自动退出
            - 一般认为，守护线程不中要或者不允许离开主线程独立运行
            - 守护线程案例能否有效果跟环境相关
            - 案例06非守护线程
			import time
			import threading

			def fun():
				print("Start fun")
				time.sleep(2)
				print("end fun")

			print("Main thread")

			t1 = threading.Thread(target=fun, args=() )
			t1.start()

			time.sleep(1)
			print("Main thread end")
            - 案例07守护线程 
			import time
			import threading

			def fun():
				print("Start fun")
				time.sleep(2)
				print("end fun")

			print("Main thread")

			t1 = threading.Thread(target=fun, args=() )
			# 社会守护线程的方法，必须在start之前设置，否则无效
			t1.setDaemon(True)
			#t1.daemon = True
			t1.start()

			time.sleep(1)
			print("Main thread end")

        - 线程常用属性
            -  threading.currentThread：返回当前线程变量
            - threading.enumerate:返回一个包含正在运行的线程的list，正在运行的线程指的是线程启动后，结束前的状态
            - threading.activeCount: 返回正在运行的线程数量，效果跟 len(threading.enumerate)相同
            - thr.setName: 给线程设置名字
            - thr.getName: 得到线程的名字
    - 直接继承自threading.Thread
        - 直接继承Thread
        - 重写run函数
        - 类实例可以直接运行
        - 案例09
			import threading
			import time

			# 1. 类需要继承自threading.Thread
			class MyThread(threading.Thread):
				def __init__(self, arg):
					super(MyThread, self).__init__()
					self.arg = arg

				# 2 必须重写run函数，run函数代表的是真正执行的功能
				def  run(self):
					time.sleep(2)
					print("The args for this class is {0}".format(self.arg))

			for i in range(5):
				t = MyThread(i)
				t.start()
				t.join()

			print("Main thread is done!!!!!!!!")
        - 案例10， 工业风案例
			import threading
			from time import sleep, ctime

			loop = [4,2]

			class ThreadFunc:

			    def __init__(self, name):
				self.name = name

			    def loop(self, nloop, nsec):
				'''
				:param nloop: loop函数的名称
				:param nsec: 系统休眠时间
				:return:
				'''
				print('Start loop ', nloop, 'at ', ctime())
				sleep(nsec)
				print('Done loop ', nloop, ' at ', ctime())

			def main():
			    print("Starting at: ", ctime())

			    # ThreadFunc("loop").loop 跟一下两个式子相等：
			    # t = ThreadFunc("loop")
			    # t.loop
			    # 以下t1 和  t2的定义方式相等
			    t = ThreadFunc("loop")
			    t1 = threading.Thread( target = t.loop, args=("LOOP1", 4))
			    # 下面这种写法更西方人，工业化一点
			    t2 = threading.Thread( target = ThreadFunc('loop').loop, args=("LOOP2", 2))

			    # 常见错误写法
			    #t1 = threading.Thread(target=ThreadFunc('loop').loop(100,4))
			    #t2 = threading.Thread(target=ThreadFunc('loop').loop(100,2))

			    t1.start()
			    t2.start()

			    t1.join( )
			    t2.join()


			    print("ALL done at: ", ctime())


			if __name__ == '__main__':
			    main()
				
- 共享变量
    - 共享变量： 当多个现成同时访问一个变量的时候，会产生共享变量的问题
    - 案例11
			import threading

			sum = 0
			loopSum = 1000000

			def myAdd():
				global  sum, loopSum
				for i in range(1, loopSum):
					sum += 1

			def myMinu():
				global  sum, loopSum
				for i in range(1, loopSum):
					sum -= 1

			if __name__ == '__main__':
				print("Starting ....{0}".format(sum))

				# 开始多线程的实例，看执行结果是否一样
				t1 = threading.Thread(target=myAdd, args=())
				t2 = threading.Thread(target=myMinu, args=())

				t1.start()
				t2.start()

				t1.join()
				t2.join()

				print("Done .... {0}".format(sum))
				
				
    - 解决变量：锁，信号灯，
    - 锁（Lock）：
        - 是一个标志，表示一个线程在占用一些资源
        - 使用方法
            - 上锁
            - 使用共享资源，放心的用
            - 取消锁，释放锁
        - 案例12
			import threading

			sum = 0
			loopSum = 1000000


			lock = threading.Lock()


			def myAdd():
				global  sum, loopSum

				for i in range(1, loopSum):
					# 上锁，申请锁
					lock.acquire()
					sum += 1
					# 释放锁
					lock.release()


			def myMinu():
				global  sum, loopSum
				for i in range(1, loopSum):
					lock.acquire()
					sum -= 1
					lock.release()

			if __name__ == '__main__':
				print("Starting ....{0}".format(sum))

				# 开始多线程的实例，看执行结果是否一样
				t1 = threading.Thread(target=myAdd, args=())
				t2 = threading.Thread(target=myMinu, args=())

				t1.start()
				t2.start()

				t1.join()
				t2.join()

				print("Done .... {0}".format(sum))

        - 锁谁： 哪个资源需要多个线程共享，锁哪个
        - 理解锁：锁其实不是锁住谁，而是一个令牌
    - 线程安全问题：
        - 如果一个资源/变量，他对于多线程来讲，不用加锁也不会引起任何问题，则称为线程安全
        - 线程不安全变量类型： list, set, dict
        - 线程安全变量类型： queue
    - 生产者消费者问题
        - 一个模型，可以用来搭建消息队列， 
        - queue是一个用来存放变量的数据结构，特点是先进先出，内部元素排队，可以理解成一个特殊的list
			#encoding=utf-8
			import threading
			import time

			# Python2
			# from Queue import Queue

			# Python3
			import queue


			class Producer(threading.Thread):
			    def run(self):
				global queue
				count = 0
				while True:
				    # qsize返回queue内容长度
				    if queue.qsize() < 1000:
					for i in range(100):
					    count = count +1
					    msg = '生成产品'+str(count)
					    # put是网queue中放入一个值
					    queue.put(msg)
					    print(msg)
				    time.sleep(0.5)


			class Consumer(threading.Thread):
			    def run(self):
				global queue
				while True:
				    if queue.qsize() > 100:
					for i in range(3):
					    # get是从queue中取出一个值
					    msg = self.name + '消费了 '+queue.get()
					    print(msg)
				    time.sleep(1)


			if __name__ == '__main__':
			    queue = queue.Queue()

			    for i in range(500):
				queue.put('初始产品'+str(i))
			    for i in range(2):
				p = Producer()
				p.start()
			    for i in range(5):
				c = Consumer()
				c.start()
		
    - 死锁问题, 案例14
			import threading
			import time

			lock_1 = threading.Lock()
			lock_2 = threading.Lock()

			def func_1():
			   print("func_1 starting.........")
			   lock_1.acquire()
			   print("func_1 申请了 lock_1....")
			   time.sleep(2)
			   print("func_1 等待 lock_2.......")
			   lock_2.acquire()
			   print("func_1 申请了 lock_2.......")

			   lock_2.release()
			   print("func_1 释放了 lock_2")

			   lock_1.release()
			   print("func_1 释放了 lock_1")

			   print("func_1 done..........")


			def func_2():
			   print("func_2 starting.........")
			   lock_2.acquire()
			   print("func_2 申请了 lock_2....")
			   time.sleep(4)
			   print("func_2 等待 lock_1.......")
			   lock_1.acquire()
			   print("func_2 申请了 lock_1.......")

			   lock_1.release()
			   print("func_2 释放了 lock_1")

			   lock_2.release()
			   print("func_2 释放了 lock_2")

			   print("func_2 done..........")

			if __name__ == "__main__":

			   print("主程序启动..............")
			   t1 = threading.Thread(target=func_1, args=())
			   t2 = threading.Thread(target=func_2, args=())

			   t1.start()
			   t2.start()

			   t1.join()
			   t2.join()

			   print("主程序启动..............")
				   
		
    - 锁的等待时间问题， v15
			import threading
			import time

			lock_1 = threading.Lock()
			lock_2 = threading.Lock()

			def func_1():
				print("func_1 starting.........")
				lock_1.acquire(timeout=4)
				print("func_1 申请了 lock_1....")
				time.sleep(2)
				print("func_1 等待 lock_2.......")

				rst = lock_2.acquire(timeout=2)
				if rst:
					print("func_1 已经得到锁 lock_2")
					lock_2.release()
					print("func_1 释放了锁 lock_2")
				else:
					print("func_1 注定没申请到lock_2.....")

				lock_1.release()
				print("func_1 释放了 lock_1")

				print("func_1 done..........")


			def func_2():
				print("func_2 starting.........")
				lock_2.acquire()
				print("func_2 申请了 lock_2....")
				time.sleep(4)
				print("func_2 等待 lock_1.......")
				lock_1.acquire()
				print("func_2 申请了 lock_1.......")

				lock_1.release()
				print("func_2 释放了 lock_1")

				lock_2.release()
				print("func_2 释放了 lock_2")

				print("func_2 done..........")

			if __name__ == "__main__":

				print("主程序启动..............")
				t1 = threading.Thread(target=func_1, args=())
				t2 = threading.Thread(target=func_2, args=())

				t1.start()
				t2.start()

				t1.join()
				t2.join()

				print("主程序结束..............")

    - semphore
        - 允许一个资源最多由几个多线程同时使用
        - v16
			import threading
			import time

			# 参数定义最多几个线程同时使用资源
			semaphore = threading.Semaphore(3)

			def func():
			    if semaphore.acquire():
				for i in range(5):
				    print(threading.currentThread().getName() + ' get semaphore')
				time.sleep(15)
				semaphore.release()
				print(threading.currentThread().getName() + ' release semaphore')


			for i in range(8):
			    t1 = threading.Thread(target=func)
			    t1.start()
				
    - threading.Timer
        - 案例 17
			import threading
			import time

			def func():
			    print("I am running.........")
			    time.sleep(4)
			    print("I am done......")



			if __name__ == "__main__":
			    t = threading.Timer(6, func)
			    t.start()

			    i = 0
			    while True:
				print("{0}***************".format(i))
				time.sleep(3)
				i += 1

        - Timer是利用多线程，在指定时间后启动一个功能
        
    - 可重入锁
        - 一个锁，可以被一个线程多次申请
        - 主要解决递归调用的时候，需要申请锁的情况
        - 案例18
			import threading
			import time

			class MyThread(threading.Thread):
			    def run(self):
				global num
				time.sleep(1)

				if mutex.acquire(1):
				    num = num+1
				    msg = self.name+' set num to '+str(num)
				    print(msg)
				    mutex.acquire()
				    mutex.release()
				    mutex.release()

			num = 0

			mutex = threading.RLock()


			def testTh():
			    for i in range(5):
				t = MyThread()
				t.start()



			if __name__ == '__main__':
			    testTh()
        
# 线程替代方案
-  subprocess
    - 完全跳过线程，使用进程
    - 是派生进程的主要替代方案
    - python2.4后引入
- multiprocessiong
    - 使用threadiing借口派生，使用子进程
    - 允许为多核或者多cpu派生进程，接口跟threading非常相似
    - python2.6
    
- concurrent.futures
    - 新的异步执行模块
    - 任务级别的操作
    - python3.2后引入
# 多进程
- 进程间通讯(InterprocessCommunication, IPC )
- 进程之间无任何共享状态
- 进程的创建
    - 直接生成Process实例对象， 案例19
			import multiprocessing
			from time import sleep, ctime


			def clock(interval):
			    while True:
				print("The time is %s" % ctime())
				sleep(interval)



			if __name__ == '__main__':
			    p = multiprocessing.Process(target = clock, args = (5,))
			    p.start()

			    while True:
				print('sleeping.......')
				sleep(1)


    - 派生子类， 案例20
			import multiprocessing
			from time import sleep, ctime


			class ClockProcess(multiprocessing.Process):
			    '''
			    两个函数比较重要
			    1. init构造函数
			    2. run
			    '''

			    def __init__(self, interval):
				super().__init__()
				self.interval = interval

			    def run(self):
				while True:
				    print("The time is %s" % ctime())
				    sleep(self.interval)


			if __name__ == '__main__':
			    p = ClockProcess(3)
			    p.start()

			    while True:
				print('sleeping.......')
				sleep(1)

    
- 在os中查看pid，ppid以及他们的关系              
    - 案例21
- 生产者消费者模型
    - JoinableQueue
    - 案例22
    - 队列中哨兵的使用, 案例23 
    - 哨兵的改进， 案例24
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
