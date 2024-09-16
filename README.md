# CS 7638: Artificial Intelligence for Robotics
## Solar System (Particle Filter) Project

**Project Description**
- 完成星际任务后，需返回地球。本项目旨在练习实现用于定位太阳系中一颗人造10.2米卫星的粒子滤波器。卫星质量在1000 - 100,000千克之间。
- 卫星通过虫洞被传送至太阳系，在围绕太阳的近似圆形轨道上运行。卫星接收太阳系中行星集体引力拉力量级的测量值，但不包括太阳的引力影响。
- 卫星可能在X和Y方向上偏离高达+ / - 4天文单位（AU）进入虫洞，太阳在X和Y方向上位于+ / - 0.1 AU内。
- 太阳系中的行星也围绕太阳沿逆时针方向在圆形或椭圆形轨道上运行。
- 必须在食物和资源耗尽之前的最多300天内定位自己和卫星。
- 软件解决方案限于15秒的“实际”CPU时间，与模拟的“卫星时间”不同。如果定位系统超过100 - 200天才能确定卫星位置，可能需要改进定位算法。
- 重力计传感器给出卫星上来自每个行星的引力加速度总和的（有噪声）量级，公式为$v = \left|\sum_{p = 1}^{n} \frac{G * M_{p}}{r_{p}^{2}}\right|$，其中v是重力量级，n是行星数量，G是引力常数，$M_{p}$是行星p的质量，$r^{-}$是从卫星到行星的向量。

**相关文件**
- `body.py`文件（不应修改，但可检查或导入）实现了模拟行星。
- `solar_system.py`文件（不应修改，但可检查或导入）包含太阳和行星的模型。
- `satellite.py`文件（不应修改，但可检查或导入）可用于模拟卫星。
- `solar_locator.py`文件包含两个必须实现的函数，且是唯一应提交给GradeScope的文件。

**Part A**
- 函数`estimate_next_pos`必须根据重力计测量值以及卫星下一次运动的距离和转向来确定卫星的下一个位置。如果估计位置与目标卫星的实际（x，y）位置相差小于0.01 AU，则成功，测试用例结束。
- 该函数每天（时间步）调用一次，每次调用会收到一个额外的数据点。可能需要整合多次调用此函数的信息才能正确估计卫星位置。“OTHER”变量可用于存储希望在下一次调用函数时返回的数据。
- 粒子滤波器可能包括以下步骤：
    - **初始化**：需要多少粒子才能覆盖目标卫星？
    - **重要性权重**：sigma参数如何影响高斯分布的概率密度函数和粒子的权重？
    - **重采样**：每个时间步应保留多少粒子，粒子更多或更少的优缺点是什么？
    - **模糊**：应具有多少位置模糊？应模糊多少百分比的粒子？
    - **利用自行车运动模型模拟目标卫星的运动**
    - **估计**

**Part B**
- 函数`next_angle`的目标是使用不同的目标卫星测量信息再次定位卫星，并设置从卫星向母星发送无线电消息的角度。
- 与Part A中每个时间步只进行一次重力计测量不同，Part B将在每个时间步提供多个百分比照明测量。卫星将按照离太阳最近到最远的顺序对每个行星进行百分比照明读数。
- 百分比照明描述了行星体2D表面在阳光下的比例。相位角是太阳 - 行星 - 卫星之间的角度，用于计算百分比照明。当太阳 - 卫星 - 行星成一线时，相位角为0度，行星看起来像满月，行星将被100％照亮。当太阳 - 行星 - 卫星成一线时，相位角为180度，行星看起来像新月，行星将被0％照亮。
- 公式为${PERCENT\_ILLUMINATION} = 50\times (1 + \cos{(PHASE\_ ANGLE)})$。
- 定位卫星后，必须向母星（太阳系中最后/最外层的行星）发送SOS消息，以便他们知道来接你。每个消息包含部分位置和轨迹信息。需要发送10个消息才能完全传输此数据。当母星收到10个消息时，测试将结束。
- `next_angle`函数将以弧度返回从卫星到母星的绝对角度以及卫星的预测位置。

**提交作业**
请参考教学大纲的“Online Grading”部分。

**计算分数**
- 测试用例是随机生成的，由于使用随机数，粒子滤波器在系统的不同运行中可能表现不同。目标是能够生成一个通常能够解决测试用例的粒子滤波器系统，而不是在每种情况下都完美的系统。
- 每个成功的测试用例将获得7分，尽管总共有20个测试用例（Part A有10个，Part B有10个）。这意味着只需要成功完成20个测试用例中的15个即可在本作业中获得满分。最高分数将上限为100，尽管如果能够解决超过15个测试用例，可以炫耀一下。

**测试代码**
- 提供了10个测试用例样本，前两个测试用例比实际评分用例更容易，因为它们没有测量噪声。这些测试用例旨在允许测试模拟的某个方面。测试用例3 - 10更能代表用于评分项目的测试用例。
- 要在终端上运行提供的测试用例：`python testing_suite_full.py`。
- 将使用10个不同的“秘密”测试用例对代码进行评分，这些测试用例与提供的公开测试用例相似，但不完全匹配。这些“秘密”测试用例是使用提供的`generate_params_planet.py`文件生成的。鼓励使用此文件生成其他测试用例来测试代码。
- 提供的测试套件类似于用于评分项目的测试套件，但需要开发其他测试用例来完全验证代码。鼓励在Ed上与其他学生分享测试用例（仅测试用例）。
- 默认情况下，测试套件使用多进程来强制执行超时。某些开发工具可能无法在启用多进程的情况下正常工作。在这种情况下，可以通过将`DEBUGGING_SINGLE_PROCESS`标志设置为True来禁用多进程。请注意，这也将禁用超时，因此如果滤波器不收敛，可能需要手动停止测试用例。
- 应确保代码在给定的每个测试用例以及自己设计的广泛其他测试用例上始终成功，因为每个评分测试用例只会运行代码一次。对于每个测试用例，代码必须在规定的时间限制（15秒）内完成执行，否则将不会获得学分。请注意，评分机器的功率相对较低，因此可能需要将本地时间限制设置为3秒，以确保不会超过评分机器上的CPU限制。

**学术诚信**
- 必须独自编写本项目的代码。虽然可以有限地使用外部资源，但必须引用在工作中使用的任何此类资源（例如，应使用注释表示从StackOverflow、讲座视频等获得的代码片段）。
- 不得在工作中使用其他人的代码。将使用代码相似性检测软件识别可疑代码，并将任何潜在事件提交给学生诚信办公室进行调查。此外，不得将工作发布在可公开访问的存储库中；这也可能导致违反荣誉准则（如果另一名学生提交了你的代码）。（考虑使用GT提供的Github存储库或默认不公开共享的Bitbucket等存储库。）

**常见问题（F.A.Q.）**
- **Q**：如何简化这个问题以便更容易思考？
    - **A**：查看此视频，该视频使用粒子滤波器解决了一个与该问题有很多相似之处的1D问题：Particle Filter explained without equations 
- **Q**：确定可以使用粒子滤波器解决这个问题吗？
    - **A**：是的
- **Q**：什么是模糊（fuzzing）？
    - **A**：模糊是扰动（一定百分比）粒子的过程，目的是使假设多样化（覆盖更广泛的搜索区域）。模糊也称为抖动或粗糙化（有时称为抖动）。在这些论文中讨论：Sample Impoverishment, PF: Tutorial, Roughening Methods
- **Q**：如何打开可视化？
    - **A**：在`testing_suite_full.py`的顶部附近，将`PLOT_PARTICLES = True`。当`PLOT_PARTICLES`设置为True时，将呈现可视化。在使用可视化进行调试时，可能还需要将`DEBUGGING_SINGLE_PROCESS = True`设置为允许测试用例运行超过“TIME_LIMIT”。
- **Q**：可视化中的每个颜色/对象是什么？
    - **A**：红色三角形是目标卫星。白色三角形是粒子。青色正方形是卫星的估计（x，y）。石灰圆圈是行星，带有天蓝色轨迹的品红色圆圈是母星。
- **Q**：可以暂停可视化吗？
    - **A**：可视化右上角有一个暂停按钮，将暂停屏幕`PAUSE_DURATION`秒。要暂停第一步，请设置`PAUSE_FIRST = True`。频繁暂停和/或长时间暂停可能需要调整“TIME_LIMIT”。
- **Q**：如何更改`TIME_LIMIT`？
    - **A**：在`testing_suite_full.py`文件的顶部附近有其他全大写变量。它们默认设置为评分者使用的相同值，但可以切换True / False值以启用/禁用详细日志记录和可视化，并增加超时值（日志记录和可视化会减慢速度）。如果计算机比Gradescope更快并且迭代更多时间步，可以考虑在开发解决方案时减少`TIME_LIMIT`。
- **Q**：为什么本地分数与Gradescope分数不同？
    - **A**：gradescope测试用例与`testing_suite_full.py`中提供的学生测试用例不同。gradescope机器可能比计算机慢或快，导致在超时之前迭代更多或更少的时间步。鼓励尽早并经常提交 - 本地计算机上失败的解决方案在Gradescope上可能表现更好，反之亦然。
- **Q**：为什么可视化打开与关闭时本地得到不同的结果？
    - **A**：由于可视化会减慢速度，关闭可视化时解决方案可能会通过更多时间步，而这些额外的时间步可能是解决方案成功的地方。

```python

# These import statements give you access to library functions which you may
# (or may not?) want to use.
import random
import time
from math import *
from body import *
from solar_system import *
from satellite import *


def estimate_next_pos(gravimeter_measurement, get_theoretical_gravitational_force_at_point, distance, steering, other=None):
    """
    Estimate the next (x,y) position of the satelite.
    This is the function you will have to write for part A.
    :param gravimeter_measurement: float
        A floating point number representing
        the measured magnitude of the gravitation pull of all the planets
        felt at the target satellite at that point in time.
    :param get_theoretical_gravitational_force_at_point: Func
        A function that takes in (x,y) and outputs a float representing the magnitude of the gravitation pull from
        of all the planets at that (x,y) location at that point in time.
    :param distance: float
        The target satellite's motion distance
    :param steering: float
        The target satellite's motion steering
    :param other: any
        This is initially None, but if you return an OTHER from
        this function call, it will be passed back to you the next time it is
        called, so that you can use it to keep track of important information
        over time. (We suggest you use a dictionary so that you can store as many
        different named values as you want.)
    :return:
        estimate: Tuple[float, float]. The (x,y) estimate of the target satellite at the next timestep
        other: any. Any additional information you'd like to pass between invocations of this function
        optional_points_to_plot: List[Tuple[float, float, float]].
            A list of tuples like (x,y,h) to plot for the visualization
    """
    # time.sleep(1)  # uncomment to pause for the specified seconds each timestep

    # example of how to get the gravity magnitude at a point in the solar system:
    gravity_magnitude = get_theoretical_gravitational_force_at_point(-1*AU, 1*AU)

    # TODO - remove this canned answer which makes this template code
    # pass one test case once you start to write your solution....
    xy_estimate = (139048139368.39096, -2225218287.6720667)

    # You may optionally also return a list of (x,y,h) points that you would like
    # the PLOT_PARTICLES=True visualizer to plot for visualization purposes.
    # If you include an optional third value, it will be plotted as the heading
    # of your particle.
    optional_points_to_plot = [(1*AU, 1*AU), (2*AU, 2*AU), (3*AU, 3*AU)]  # Sample (x,y) to plot
    optional_points_to_plot = [(1*AU, 1*AU, 0.5), (2*AU, 2*AU, 1.8), (3*AU, 3*AU, 3.2)]  # (x,y,heading)

    return xy_estimate, other, optional_points_to_plot


def next_angle(solar_system, percent_illuminated_measurements, percent_illuminated_sense_func,
               distance, steering, other=None):
    """
    Gets the next angle at which to send out an sos message to the home planet,
    the last planet in the solar system.
    This is the function you will have to write for part B.
    :param solar_system: SolarSystem
        A model of the solar system containing the sun and planets as Bodys (contains positions, velocities, and masses)
        Planets are listed in order from closest to furthest from the sun
    :param percent_illuminated_measurements: List[float]
        A list of floating point number from 0 to 100 representing
        the measured percent illumination of each planet in order from closest to furthest to sun
        as seen by the target satellite.
    :param percent_illuminated_sense_func: Func
        A function that takes in (x,y) and outputs the list of percent illuminated measurements of each planet
        as would be seen by satellite at that (x,y) location.
    :param distance: float
        The target satellite's motion distance
    :param steering: float
        The target satellite's motion steering
    :param other: any
        This is initially None, but if you return an OTHER from
        this function call, it will be passed back to you the next time it is
        called, so that you can use it to keep track of important information
        over time. (We suggest you use a dictionary so that you can store as many
        different named values as you want.)
    :return:
        bearing: float. The absolute angle from the satellite to send an sos message between -pi and pi
        xy_estimate: Tuple[float, float]. The (x,y) estimate of the target satellite at the next timestep
        other: any. Any additional information you'd like to pass between invocations of this function
        optional_points_to_plot: List[Tuple[float, float, float]].
            A list of tuples like (x,y,h) to plot for the visualization
    """

    # At what angle to send an SOS message this timestep
    bearing = 0.0
    xy_estimate = (110172640485.32968, -66967324464.19617)

    # You may optionally also return a list of (x,y) or (x,y,h) points that
    # you would like the PLOT_PARTICLES=True visualizer to plot.
    optional_points_to_plot = [ (1*AU,1*AU), (2*AU,2*AU), (3*AU,3*AU) ]  # Sample plot points

    return bearing, xy_estimate, other, optional_points_to_plot


def who_am_i():
    # Please specify your GT login ID in the whoami variable (ex: jsmith224).
    whoami = ''
    return whoami
```
# CS7638 Solar System Particle Filters Project

# CS Tutor | 计算机编程辅导 | Code Help | Programming Help

# WeChat: cstutorcs

# Email: tutorcs@163.com

# QQ: 749389476

# 非中介, 直接联系程序员本人
