.. _micropython:

===========
Micropython
===========

What is Micropython?
--------------------

`Micropython <http://micropython.org/>`__ is Python for
microcontrollers. Using Micropython, you can write Python3 code and run
it even on a bare metal architecture with limited resources.


Highlights of Micropython
~~~~~~~~~~~~~~~~~~~~~~~~~

- **Compact**: Fits and runs within just 256k of code space and 16k of RAM. No OS is needed, although you
  can also run it with an OS, if you want.
- **Compatible**: Strives to be as compatible as possible with normal Python (known as CPython).
- **Versatile**: Supports many architectures (x86, x86-64, ARM, ARM Thumb, Xtensa).
- **Interactive**: No need for the compile-flash-boot cycle. With the REPL (interactive prompt) you can type
  commands and execute them immediately, run scripts, etc.
- **Popular**: Many platforms are supported. The user base is growing bigger. Notable forks:

  - `MicroPython <https://github.com/micropython/micropython>`__
  - `CircuitPython <https://github.com/adafruit/circuitpython>`__
  - `MicroPython_ESP32_psRAM_LoBo <https://github.com/loboris/MicroPython_ESP32_psRAM_LoBo>`__

- **Embedded Oriented**: Comes with modules specifically for embedded systems, such as the
  `machine module <https://docs.micropython.org/en/latest/library/machine.html#classes>`__
  for accessing low-level hardware (I/O pins, ADC, UART, SPI, I2C, RTC, Timers etc.)

--------------


Why Micropython + LVGL?
-----------------------

Micropython `does not have a good native high-level GUI library <https://forum.micropython.org/viewtopic.php?f=18&t=5543>`__.
LVGL is an `Object-Oriented Component Based <https://blog.lvgl.io/2018-12-13/extend-lvgl-objects>`__
high-level GUI library, which seems to be a natural candidate to map into a higher level language, such as Python.
LVGL is implemented in C and its APIs are in C.


Here are some advantages of using LVGL in Micropython:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Develop GUI in Python, a very popular high level language. Use paradigms such as Object-Oriented Programming.
- Usually, GUI development requires multiple iterations to get things right. With C, each iteration consists of
  **``Change code`` > ``Build`` > ``Flash`` > ``Run``**. In Micropython it's just
  **``Change code`` > ``Run``** ! You can even run commands interactively using the
  `REPL <https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop>`__ (the interactive prompt)

Micropython + LVGL could be used for:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Fast prototyping GUI.
- Shortening the cycle of changing and fine-tuning the GUI.
- Modelling the GUI in a more abstract way by defining reusable composite objects, taking advantage of Python's language features
  such as Inheritance, Closures, List Comprehension, Generators, Exception Handling, Arbitrary Precision Integers and others.
- Make LVGL accessible to a larger audience. No need to know C to create a nice GUI on an embedded system. This goes well with
  `CircuitPython vision <https://learn.adafruit.com/welcome-to-circuitpython/what-is-circuitpython>`__.
  CircuitPython was designed with education in mind, to make it easier for new or inexperienced users to get started with
  embedded development.
- Creating tools to work with LVGL at a higher level (e.g. drag-and-drop designer).

--------------


So what does it look like?
--------------------------

It's very much like the C API, but Object-Oriented for LVGL components.

Let's dive right into an example!


A simple example
~~~~~~~~~~~~~~~~

.. code:: python

   import lvgl as lv
   lv.init()
   scr = lv.obj()
   btn = lv.btn(scr)
   btn.align(lv.ALIGN.CENTER, 0, 0)
   label = lv.label(btn)
   label.set_text('Hello World!')
   lv.scr_load(scr)


How can I use it?
-----------------

Online Simulator
~~~~~~~~~~~~~~~~

If you want to experiment with LVGL + Micropython without downloading
anything - you can use our online simulator! It's a fully functional
LVGL + Micropython that runs entirely in the browser and allows you to
edit a python script and run it.

`Click here to experiment on the online simulator <https://sim.lvgl.io/>`__

Many `LVGL examples <https://docs.lvgl.io/master/examples.html>`__ are available also for Micropython. Just click the link!


PC Simulator
~~~~~~~~~~~~

Micropython is ported to many platforms. One notable port is "unix", which allows you to build and run Micropython
(+LVGL) on a Linux machine. (On a Windows machine you might need Virtual Box or WSL or MinGW or Cygwin etc.)

`Click here to know more information about building and running the unix port <https://github.com/lvgl/lv_micropython>`__


Embedded Platforms
~~~~~~~~~~~~~~~~~~

In the end, the goal is to run it all on an embedded platform. Both Micropython and LVGL can be used on many embedded
architectures. `lv_micropython <https://github.com/lvgl/lv_micropython>`__ is a fork of Micropython+LVGL and currently
supports Linux, ESP32, STM32 and RP2. It can be ported to any other platform supported by Micropython.

- You would also need display and input drivers. You can either use one of the existing drivers provided with lv_micropython,
  or you can create your own input/display drivers for your specific hardware.
- Drivers can be implemented either in C as a Micropython module, or in pure Python!

lv_micropython already contains these drivers:

- Display drivers:

  - SDL on Linux
  - ESP32 specific:

    - ILI9341
    - ILI9488
    - GC9A01
    - ST7789
    - ST7735

  - Generic (pure Python):

    - ILI9341
    - ST7789
    - ST7735

- Input drivers:

  - SDL
  - XPT2046
  - FT6X36
  - ESP32 ADC with resistive touch


Where can I find more information?
----------------------------------

- ``lv_micropython`` `README <https://github.com/lvgl/lv_micropython>`__
- ``lv_binding_micropython`` `README <https://github.com/lvgl/lv_binding_micropython>`__
- The `LVGL micropython forum <https://forum.lvgl.io/c/micropython>`__ (Feel free to ask anything!)
- At Micropython: `docs <http://docs.micropython.org/en/latest/>`__ and `forum <https://forum.micropython.org/>`__
- `Blog Post <https://blog.lvgl.io/2019-02-20/micropython-bindings>`__, a little outdated.


The Micropython Binding is auto generated!
------------------------------------------

- LVGL is a git submodule inside `lv_micropython <https://github.com/lvgl/lv_micropython>`__
  (LVGL is a git submodule of `lv_binding_micropython <https://github.com/lvgl/lv_binding_micropython>`__
  which is itself a submodule of `lv_micropython <https://github.com/lvgl/lv_micropython>`__).
- When building lv_micropython, the public LVGL C API is scanned and Micropython API is auto-generated. That means that
  lv_micropython provides LVGL API for **any** LVGL version, and generally does not require code changes as LVGL evolves.


LVGL C API Coding Conventions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To support the auto-generation of the Python API, the LVGL C API must
follow some coding conventions:

- Use ``enum``\ s instead of macros. If inevitable to use ``define``\ s
  export them with :cpp:expr:`LV_EXPORT_CONST_INT(defined_value)` right after the ``define``.
- In function arguments use ``type name[]`` declaration for array parameters instead of :cpp:expr:`type * name`
- Use typed pointers instead of :cpp:expr:`void *` pointers
- Widget constructor must follow the ``lv_<widget_name>_create(lv_obj_t * parent)`` pattern.
- Widget members function must start with ``lv_<modul_name>`` and should receive :cpp:expr:`lv_obj_t *` as first
  argument which is a pointer to widget object itself.
- ``struct`` APIs should follow the widgets' conventions. That is to receive a pointer to the ``struct`` as the
  first argument, and the prefix of the ``struct`` name should be used as the prefix of the
  function name too (e.g. :cpp:expr:`lv_disp_set_default(lv_disp_t * disp)`)
- Functions and ``struct``\ s which are not part of the public API must begin with underscore in order to mark them as "private".
- Argument must be named in H files too.
- Do not ``malloc`` into a static or global variables. Instead declare the variable in :c:macro:`LV_ITERATE_ROOTS`
  list in ``lv_gc.h`` and mark the variable with :cpp:expr:`GC_ROOT(variable)` when it's used. **See** :ref:`memory_management`
- To register and use callbacks one of the following needs to be followed.  **See** :ref:`callbacks`

   - Pass a pointer to a ``struct`` as the first argument of both the registration function and the callback. That
     ``struct`` must contain ``void * user_data`` field.
   - The last argument of the registration function must be ``void * user_data`` and the same ``user_data``
     needs to be passed as the last argument of the callback.

Most of these rules are simple and straightforward but there are two related concepts that worth a deeper look:
:ref:`memory_management` and :ref:`callbacks`.

.. _memory_management:

Memory Management
~~~~~~~~~~~~~~~~~

| When LVGL runs in Micropython, all dynamic memory allocations (:cpp:func:`lv_malloc`) are handled by Micropython's memory
  manager which is `garbage-collected <https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)>`__ (GC).
| To prevent GC from collecting memory prematurely, all dynamic allocated RAM must be reachable by GC.
| GC is aware of most allocations, except from pointers on the `Data Segment <https://en.wikipedia.org/wiki/Data_segment>`__:

    - Pointers which are global variables
    - Pointers which are static global variables
    - Pointers which are static local variables

Such pointers need to be defined in a special way to make them reachable by GC


Identify The Problem
^^^^^^^^^^^^^^^^^^^^

Problem happens when an allocated memory's pointer (return value of :cpp:func:`lv_malloc`) is stored only in either **global**,
**static global** or **static local** pointer variable and not as part of a previously allocated ``struct`` or other variable.


Solve The Problem
^^^^^^^^^^^^^^^^^

- Replace the global/static local var with :cpp:expr:`LV_GC_ROOT(_var)`
- Include ``lv_gc.h`` on files that use :c:macro:`LV_GC_ROOT`
- Add ``_var`` to :c:macro:`LV_ITERATE_ROOTS` on ``lv_gc.h``

Example
^^^^^^^

https://github.com/lvgl/lvgl/commit/adced46eccfa0437f84aa51aedca4895cc3c679c


More Information
^^^^^^^^^^^^^^^^

- `In the README <https://github.com/lvgl/lv_binding_micropython#memory-management>`__
- `In the Blog <https://blog.lvgl.io/2019-02-20/micropython-bindings#i-need-to-allocate-a-littlevgl-struct-such-as-style-color-etc-how-can-i-do-that-how-do-i-allocatedeallocate-memory-for-it>`__

.. _callbacks:

Callbacks
~~~~~~~~~

In C a callback is just a function pointer. But in Micropython we need to register a *Micropython callable object* for each
callback. Therefore in the Micropython binding we need to register both a function pointer and a Micropython object for every callback.

Therefore we defined a **callback convention** for the LVGL C API that expects lvgl headers to be defined in a certain
way. Callbacks that are declared according to the convention would allow the binding to register a Micropython object
next to the function pointer when registering a callback, and access that object when the callback is called.

- The basic idea is that we have ``void * user_data`` field that is used automatically by the Micropython Binding
  to save the *Micropython callable object* for a callback. This field must be provided when registering the function
  pointer, and provided to the callback function itself.
- Although called "user_data", the user is not expected to read/write that field. Instead, the Micropython glue code uses
  ``user_data`` to automatically keep track of the Micropython callable object. The glue code updates it when the callback
  is registered, and uses it when the callback is called in order to invoke a call to the original callable object.

There are a few options for defining a callback in LVGL C API:

- Option 1: ``user_data`` in a struct

  - There's a struct that contains a field called ``void * user_data``

    - A pointer to that struct is provided as the **first** argument of a callback registration function
    - A pointer to that struct is provided as the **first** argument of the callback itself

- Option 2: ``user_data`` as a function argument

  - A parameter called ``void * user_data`` is provided to the registration function as the **last** argument

    - The callback itself receives ``void *`` as the **last** argument

- Option 3: both callback and ``user_data`` are struct fields

  - The API exposes a struct with both function pointer member and ``user_data`` member

    - The function pointer member receives the same struct as its **first** argument

In practice it's also possible to mix these options, for example provide a struct pointer when registering a callback
(option 1) and provide ``user_data`` argument when calling the callback (options 2),
**as long as the same ``user_data`` that was registered is passed to the callback when it's called**.

Examples
^^^^^^^^

- :cpp:type:`lv_anim_t` contains ``user_data`` field. :cpp:func:`lv_anim_set_path_cb`
  registers `path_cb` callback. Both ``lv_anim_set_path_cb`` and :cpp:type:`lv_anim_path_cb_t`
  receive :cpp:type:`lv_anim_t` as their first argument
- ``path_cb`` field can also be assigned directly in the Python code because it's a member
  of :cpp:type:`lv_anim_t` which contains ``user_data`` field, and :cpp:type:`lv_anim_path_cb_t`
  receive :cpp:type:`lv_anim_t` as its first argument.
- :cpp:func:`lv_imgfont_create` registers ``path_cb`` and receives ``user_data`` as the last
  argument. The callback :cpp:func:`lv_imgfont_get_path_cb_t` also receives the ``user_data`` as the last argument.

.. _more-information-1:

More Information
^^^^^^^^^^^^^^^^

- In the `Blog <https://blog.lvgl.io/2019-08-05/micropython-pure-display-driver#using-callbacks>`__
  and in the `README <https://github.com/lvgl/lv_binding_micropython#callbacks>`__
- `[v6.0] Callback conventions  #1036 <https://github.com/lvgl/lvgl/issues/1036>`__
- Various discussions: `here <https://github.com/lvgl/lvgl/pull/3294#issuecomment-1184895335>`__
  and `here <https://github.com/lvgl/lvgl/issues/1763#issuecomment-762247629>`__
  and`here <https://github.com/lvgl/lvgl/issues/316#issuecomment-467221587>`__
