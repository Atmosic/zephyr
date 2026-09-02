:orphan:

..
  See
  https://docs.zephyrproject.org/latest/releases/index.html#migration-guides
  for details of what is supposed to go into this document.

.. _migration_4.5:

Migration guide to Zephyr v4.5.0 (Working Draft)
################################################

This document describes the changes required when migrating your application from Zephyr v4.4.0 to
Zephyr v4.5.0.

Any other changes (not directly related to migrating applications) can be found in
the :ref:`release notes<zephyr_4.5>`.

.. contents::
    :local:
    :depth: 2

Common
******

Build System
************

Kernel
******

Boards
******

Device Drivers and Devicetree
*****************************

.. Group contents in this section by subsystem, e.g.:
..
.. ADC
.. ===
..
.. ...

.. zephyr-keep-sorted-start re(^\w) ignorecase


.. zephyr-keep-sorted-stop

Bluetooth
*********

Bluetooth Host
==============

* The ``le_param_updated`` callback in :c:struct:`bt_conn_cb` is no longer invoked when a
  connection parameter update fails (i.e. the LE Connection Update Complete event reports a
  non-zero status). Previously it was called unconditionally, reporting the unchanged
  connection parameters with no error indication, which could not be distinguished from a
  successful update. Applications that need to be notified about rejected, application-initiated
  parameter updates should enable :kconfig:option:`CONFIG_BT_USER_CONN_PARAM_REJECTED` and
  implement the new ``le_param_update_rejected`` callback.

Networking
**********

Other subsystems
****************

Modules
*******

Architectures
*************
