---
layout: post
title:  "Run Python Unit Tests Automatically with Sniffer"
date:   2014-12-03 21:00:00
categories: code, python, testing
---

I was recently writing a couple of plug-ins for [Ansible](https://github.com/ansible/ansible), I only want to use
standard libraries so everything works right out of the box on build servers etc.

I love having my tests running automatically while developing code similar to the way
[guard](https://github.com/guard/guard) for Ruby works but I prefer to run things native to the language I'm developing
in.

[Sniffer](https://github.com/jeffh/sniffer) was the answer for me but required a custom `scent.py` file to work with
vanilla `unittest`, here's how mine looks.

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
