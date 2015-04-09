---
layout: post
title:  "Run Python Unit Tests Automatically with Sniffer"
date:   2015-04-15 11:00:00
categories: code, python, testing
---

I was recently writing a couple of plug-ins for [Ansible](https://github.com/ansible/ansible), I only want to use
standard libraries so everything works right out of the box on older build servers etc.

I love having my tests running automatically while developing code similar to the way
[guard](https://github.com/guard/guard) for Ruby works. As much as I like guard I prefer to use tools native to the
language I'm working in, that way I can easily parse unittest output etc.

After some evaluation I settled for [Sniffer](https://github.com/jeffh/sniffer), it required a custom `scent.py` file to work with
vanilla `unittest`, so here is mine, hope it saves you some time:

~~~python
import sniffer
import termstyle

# you can customize the pass/fail colors like this
pass_fg_color = termstyle.green
pass_bg_color = termstyle.bg_default
fail_fg_color = termstyle.red
fail_bg_color = termstyle.bg_default


def run_tests(*args):
        import unittest
        tests = unittest.TestLoader().discover('.', '*test.py')
        result = unittest.TextTestRunner().run(tests)
        return result.wasSuccessful()

        sniffer.Sniffer.run = run_tests
~~~
