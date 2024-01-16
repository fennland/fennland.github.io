# 欢迎

欢迎光临 [fennland](https://fennland.gitee.io). 您同样可以通过 [Github Pages](https://fennland.github.io), [Gitee Pages](https://fennland.gitee.io), 以及 [fennland 博客](https://fennland.cn) 访问本站点.

## 🏃🏻 谁是 fenn

* `👨‍🎓 学校` 华侨大学，计算机科学与技术学院，2021级计算机科学与技术1班，学生
* `🌍 位置` 湖南株洲人，现居福建厦门
* `🎂 生日` 2003年4月出生
* `👣 学习方向` 全栈开发、UniApp、Flutter、SwiftUI、Golang、Unity2D
* `🙇‍♂️ 项目经历` [华园小湘小程序](https://hquer.fennland.cn/), [一起拼](https://gitee.com/fennland/pin_demo), DDay, [DictationNow](https://github.com/fennland/dictation-tool)
* `📮 联系方式` [fennland 邮箱](mailto:fenn@fennland.cn), [WeChat 公众号](https://mp.weixin.qq.com/mp/profile_ext?action=home&__biz=Mzg2NDYzMjY0Nw==&scene=117#wechat_redirect)

## 🗺️ 什么是 fennland

```python title="what_is_fennland.py"
import re
import random

class fennland:
    def __init__(self, name, purpose):
        self.name = name
        self.purpose = purpose

    def introduce(self):
        introduction = f"This is {self.name}, which is designed to {self.purpose}."
        poem = self.generate_poem()

        clean_introduction = re.sub(r'[^\w\s]', '', introduction)

        print(clean_introduction if clean_introduction else "Sorry, I couldn't introduce myself.")
        print(poem)

    def generate_poem(self):
        subjects = ["moon", "sun", "stars", "flowers", "ocean"]
        verbs = ["shine", "dance", "twinkle", "bloom", "flow"]
        adjectives = ["beautiful", "bright", "glowing", "fragrant", "serene"]

        subject = random.choice(subjects)
        verb = random.choice(verbs)
        adjective = random.choice(adjectives)

        poem = f"The {subject} {verb} in the {adjective} night."

        return poem

if __name__ == "__main__":
    who = fennland("fennland", "write down and share some sparkling memos")
    who.introduce()
```

- 你将可以了解`fennland`最新动态，包括但不限于：
  - 心情随笔：和 WeChat 公众号同步更新的随笔文字
  - 开发笔记：在目前方向上的开发学习过程中的心得体会
  - 产品动态：产品、项目迭代记录和更新日志

- 你将可以通过留言板同`fennland`留言互动。
