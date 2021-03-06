Introduction to OpenCV.js and Tutorials {#tutorial_js_intro}
=======================================

OpenCV
------

OpenCV was created at Intel in 1999 by **Gary Bradski**. The first release came out in 2000. **Vadim Pisarevsky** joined Gary Bradski to manage Intel's Russian software OpenCV team. In 2005, OpenCV was used on Stanley; the vehicle that won the 2005 DARPA Grand Challenge. Later, its active development continued under the support of Willow Garage, with Gary Bradski and Vadim Pisarevsky leading the project. OpenCV now supports a multitude of algorithms related to Computer Vision and Machine Learning and is expanding day by day.

OpenCV supports a wide variety of programming languages such as C++, Python, Java, and JavaScript, and is available on different platforms including Windows, Linux, OS X, Android, iOS and web browsers. Interfaces for high-speed GPU operations based on CUDA and OpenCL are also under active development.

OpenCV.js
-------------

Web is the most ubiquitous open computing platform. With HTML5 standards implemented in every browser, web applications are able to render online video with HTML5 video tags, capture webcam video via WebRTC API, and access each pixel of a video frame via canvas API. With these such a wide variety of multimedia  available for use online, web developers are in need of a wide array of image and vision processing algorithms in JavaScript to build innovative applications, such as facial detection in live WebRTC streaming. This requirement is even more essential for emerging usages on web, such as Web Virtual Reality and Augmented Reality (WebVR). All of these use cases demand efficient implementations of computer-intensive vision kernels on web.

[Emscripten](http://kripken.github.io/emscripten-site) is an LLVM-to-JavaScript compiler. It takes LLVM bitcode - which can be generated from C/C++ using clang, and compiles that into asm.js, a subset of JavaScript. Asm.js is an intermediate programming language designed to allow computer software written in languages such as C to be run as web applications. Asm.js maintains performance characteristics considerably better than standard JavaScript, the typical language used for such applications. The performance of asm.js is improved by limiting language features of JavaScript to those amenable to ahead-of-time optimization and other performance improvements.

OpenCV.js is a JavaScript binding for a subset of OpenCV. It allows emerging web applications with multimedia processing to benefit from the wide variety of vision functions available in OpenCV. OpenCV.js leverages Emscripten, compiles OpenCV functions' implementation into asm.js, and exposes JavaScript APIs for web application use. The future version will support WebAssembly and other acceleration APIs available on Web.

OpenCV.js was created in Parallel Architectures and Systems Group at University of California Irvine (UCI) as a research project funded by Intel Corporation. OpenCV.js was improved and integrated into the OpenCV project as part of Google Summer of Code 2017 projects.

OpenCV.js Tutorials
-----------------------

OpenCV introduces a new set of tutorials that will guide you through various functions available in OpenCV.js. **This guide is mainly focused on OpenCV 3.x version**.

The purpose of OpenCV.js tutorials is to:
-# Help with adaptability of OpenCV in web development
-# Help the web community, developers and computer vision researchers to interactively access a variety of web-based OpenCV examples to help them understand specific vision algorithms.

Because OpenCV.js is able to run directly inside browser, the OpenCV.js tutorial web pages are intuitive and interactive. For example, using WebRTC API and evaluating JavaScript code would allow developers to change the parameters of CV functions and do live CV coding on web pages to see the results in real time.

Prior knowledge of JavaScript and web application development is recommended to understand this guide.

Contributors
------------

Below is the list of contributors of OpenCV.js bindings and tutorials.

-#  Sajjad Taheri (Google Summer of Code 2017 mentor, University of California, Irvine)
-#  Congxiang Pan (Google Summer of Code 2017 intern, Shanghai Jiao Tong University)
-#  Gang Song (Google Summer of Code 2017 intern, Shanghai Jiao Tong University)
-#  Wenyao Gan (Shanghai Jiao Tong University)
-#  Mohammad Reza Haghighat (Intel Corporation)
-#  Ningxin Hu (Intel Corporation)