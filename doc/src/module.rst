.. module:: cx_Oracle

.. _module:

****************
Module Interface
****************

.. function:: Binary(string)

   Construct an object holding a binary (long) string value.


.. function:: clientversion()

   Return the version of the client library being used as a 5-tuple. The five
   values are the major version, minor version, update number, patch number and
   port update number.

   .. note::

      This method is an extension to the DB API definition.


.. function:: Connection([user, password, dsn, mode, handle, pool, threaded, twophase, events, cclass, purity, newpassword, encoding, nencoding, module, action, clientinfo, edition, appcontext])
              connect([user, password, dsn, mode, handle, pool, threaded, twophase, events, cclass, purity, newpassword, encoding, nencoding, module, action, clientinfo, edition, appcontext])

   Constructor for creating a connection to the database. Return a
   :ref:`connection object <connobj>`. All arguments are optional and can be
   specified as keyword parameters.
  
   The dsn (data source name) is the TNS entry (from the Oracle names server or
   tnsnames.ora file) or is a string like the one returned from makedsn(). If
   only one parameter is passed, a connect string is assumed which is to be of
   the format ``user/password@dsn``, the same format accepted by Oracle
   applications such as SQL\*Plus.
  
   If the mode is specified, it must be one of :data:`~cx_Oracle.SYSDBA`,
   :data:`~cx_Oracle.SYSASM` or :data:`~cx_Oracle.SYSOPER` which are defined at
   the module level; otherwise, it defaults to the normal mode of connecting.
  
   If the handle is specified, it must be of type OCISvcCtx\* and is only of
   use when embedding Python in an application (like PowerBuilder) which has
   already made the connection.
  
   The pool argument is expected to be a :ref:`session pool object <sesspool>`
   and the use of this argument is the equivalent of calling pool.acquire().
  
   The threaded argument is expected to be a boolean expression which
   indicates whether or not Oracle should wrap accesses to connections with a
   mutex. Doing so in single threaded applications imposes a performance
   penalty of about 10-15% which is why the default is False.
  
   The twophase argument is expected to be a boolean expression which
   indicates whether or not the external_name and internal_name attributes
   should be set on the connection object to allow for two phase commit. The
   default for this value is False. Use of this argument is deprecated and will
   be removed in a future release of cx_Oracle. Instead, the application should
   set these attributes itself to an appropriate value for the application.

   The events argument is expected to be a boolean expression which indicates
   whether or not to initialize Oracle in events mode.

   The cclass argument is expected to be a string and defines the connection
   class for database resident connection pooling (DRCP).

   The purity argument is expected to be one of
   :data:`~cx_Oracle.ATTR_PURITY_NEW`, :data:`~cx_Oracle.ATTR_PURITY_SELF`, or
   :data:`~cx_Oracle.ATTR_PURITY_DEFAULT`.

   The newpassword argument is expected to be a string if specified and sets
   the password for the logon during the connection process.

   The encoding argument is expected to be a string if specified and sets the
   encoding to use for regular database strings.

   The nencoding argument is expected to be a string if specified and sets the
   national encoding to use for national character set database strings.

   The module, action and clientinfo arguments are expected to be strings, if
   specified, and set the module, action and client_info attributes on the
   connection respectively. These arguments are deprecated and will be removed
   in a future version of cx_Oracle since Oracle does not support their use
   during the creation of a connection. Instead, application context (see
   below) should be used.

   The edition argument is expected to be a string if specified and sets the
   edition to use for the session. It is only relevant if both the client and
   the server are at least Oracle Database 11.2.

   The appcontext argument is expected to be a list of 3-tuples, if specified,
   and sets the application context for the connection. Application context
   is available in the database by using the sys_context() PL/SQL method and
   can be used within a logon trigger as well as any other PL/SQL procedures.
   Each entry in the list is expected to contain three strings: the namespace,
   the name and the value.


.. function:: Cursor(connection)

   Constructor for creating a cursor.  Return a new 
   :ref:`cursor object <cursorobj>` using the connection.

   .. note::

      This method is an extension to the DB API definition.


.. function:: Date(year, month, day)

   Construct an object holding a date value.


.. function:: DateFromTicks(ticks)

   Construct an object holding a date value from the given ticks value (number
   of seconds since the epoch; see the documentation of the standard Python
   time module for details).


.. function:: makedsn(host, port, sid, [service_name])

   Return a string suitable for use as the dsn argument for
   :meth:`~cx_Oracle.connect()`. This string is identical to the strings that
   are defined by the Oracle names server or defined in the tnsnames.ora file.
   If you wish to use the service name instead of the sid, do not include a
   value for the parameter sid and use the keyword parameter service_name
   instead.

   .. note::

      This method is an extension to the DB API definition.


.. function:: SessionPool(user, password, database, min, max, increment, [connectiontype=cx_Oracle.Connection, threaded=False, getmode=cx_Oracle.SPOOL_ATTRVAL_NOWAIT, homogeneous=True, externalauth=False, encoding=None, nencoding=None])

   Create and return a :ref:`session pool object <sesspool>`. This
   allows for very fast connections to the database and is of primary use in a
   server where the same connection is being made multiple times in rapid
   succession (a web server, for example).

   If the connection type is specified, all calls to acquire() will create
   connection objects of that type, rather than the base type defined at the
   module level.

   The threaded attribute is expected to be a boolean expression which
   indicates whether Oracle should wrap accesses to connections with a mutex.
   Doing so in single threaded applications imposes a performance penalty of
   about 10-15% which is why the default is False.

   The encoding argument is expected to be a string if specified and sets the
   encoding to use for regular database strings.

   The nencoding argument is expected to be a string if specified and sets the
   national encoding to use for national character set database strings.

   .. note::

      This method is an extension to the DB API definition.


.. function:: Time(hour, minute, second)

   Construct an object holding a time value.


.. function:: TimeFromTicks(ticks)

   Construct an object holding a time value from the given ticks value (number
   of seconds since the epoch; see the documentation of the standard Python
   time module for details).


.. function:: Timestamp(year, month, day, hour, minute, second)

   Construct an object holding a time stamp value.


.. function:: TimestampFromTicks(ticks)

   Construct an object holding a time stamp value from the given ticks value
   (number of seconds since the epoch; see the documentation of the standard
   Python time module for details).



.. _constants:

Constants
=========

General
-------

.. data:: apilevel

   String constant stating the supported DB API level. Currently '2.0'.


.. data:: buildtime

   String constant stating the time when the binary was built.

   .. note::

      This constant is an extension to the DB API definition.


.. data:: paramstyle

   String constant stating the type of parameter marker formatting expected by
   the interface. Currently 'named' as in 'where name = :name'.


.. data:: threadsafety

   Integer constant stating the level of thread safety that the interface
   supports.  Currently 2, which means that threads may share the module and
   connections, but not cursors. Sharing means that a thread may use a
   resource without wrapping it using a mutex semaphore to implement resource
   locking.

   Note that in order to make use of multiple threads in a program which
   intends to connect and disconnect in different threads, the threaded
   argument to :meth:`connect()` or :meth:`SessionPool()` must be true.


.. data:: version
.. data:: __version__

   String constant stating the version of the module. Currently '|release|'.

   .. note::

      This attribute is an extension to the DB API definition.


Advanced Queuing: Delivery Modes
--------------------------------

These constants are extensions to the DB API definition. They are possible
values for the :attr:`~DeqOptions.deliverymode` attribute of the
:ref:`dequeue options object <deqoptions>` passed as the options argument to
the :meth:`Connection.deq()` method as well as the
:attr:`~EnqOptions.deliverymode` attribute of the
:ref:`enqueue options object <enqoptions>` passed as the options argument to
the :meth:`Connection.enq()` method. They are also possible values for the
:attr:`~MessageProperties.deliverymode` attribute of the
:ref:`message properties object <msgproperties>` passed as the msgproperties
argument to the :meth:`Connection.deq()` and :meth:`Connection.enq()` methods.


.. data:: MSG_BUFFERED

   This constant is used to specify that enqueue/dequeue operations should
   enqueue or dequeue buffered messages.


.. data:: MSG_PERSISTENT

   This constant is used to specify that enqueue/dequeue operations should
   enqueue or dequeue persistent messages. This is the default value.


.. data:: MSG_PERSISTENT_OR_BUFFERED

   This constant is used to specify that dequeue operations should dequeue
   either persistent or buffered messages.


Advanced Queuing: Dequeue Modes
-------------------------------

These constants are extensions to the DB API definition. They are possible
values for the :attr:`~DeqOptions.mode` attribute of the
:ref:`dequeue options object <deqoptions>`. This object is the options argument
for the :meth:`Connection.deq()` method.


.. data:: DEQ_BROWSE

   This constant is used to specify that dequeue should read the message
   without acquiring any lock on the message (eqivalent to a select statement).


.. data:: DEQ_LOCKED

   This constant is used to specify that dequeue should read and obtain a
   write lock on the message for the duration of the transaction (equivalent to
   a select for update statement).


.. data:: DEQ_REMOVE

   This constant is used to specify that dequeue should read the message and
   update or delete it. This is the default value.


.. data:: DEQ_REMOVE_NODATA

   This constant is used to specify that dequeue should confirm receipt of the
   message but not deliver the actual message content.


Advanced Queuing: Dequeue Navigation Modes
------------------------------------------

These constants are extensions to the DB API definition. They are possible
values for the :attr:`~DeqOptions.navigation` attribute of the
:ref:`dequeue options object <deqoptions>`. This object is the options argument
for the :meth:`Connection.deq()` method.


.. data:: DEQ_FIRST_MSG

   This constant is used to specify that dequeue should retrieve the first
   available message that matches the search criteria. This resets the position
   to the beginning of the queue.


.. data:: DEQ_NEXT_MSG

   This constant is used to specify that dequeue should retrieve the next
   available message that matches the search criteria. If the previous message
   belongs to a message group, AQ retrieves the next available message that
   matches the search criteria and belongs to the message group. This is the
   default.


.. data:: DEQ_NEXT_TRANSACTION

   This constant is used to specify that dequeue should skip the remainder of
   the transaction group and retrieve the first message of the next transaction
   group. This option can only be used if message grouping is enabled for the
   current queue.


Advanced Queuing: Dequeue Visibility Modes
------------------------------------------

These constants are extensions to the DB API definition. They are possible
values for the :attr:`~DeqOptions.visibility` attribute of the
:ref:`dequeue options object <deqoptions>`. This object is the options argument
for the :meth:`Connection.deq()` method.


.. data:: DEQ_IMMEDIATE

   This constant is used to specify that dequeue should perform its work as
   part of an independent transaction.


.. data:: DEQ_ON_COMMIT

   This constant is used to specify that dequeue should be part of the current
   transaction. This is the default value.


Advanced Queuing: Dequeue Wait Modes
------------------------------------

These constants are extensions to the DB API definition. They are possible
values for the :attr:`~DeqOptions.wait` attribute of the
:ref:`dequeue options object <deqoptions>`. This object is the options argument
for the :meth:`Connection.deq()` method.


.. data:: DEQ_NO_WAIT

   This constant is used to specify that dequeue not wait for messages to be
   available for dequeuing.


.. data:: DEQ_WAIT_FOREVER

   This constant is used to specify that dequeue should wait forever for
   messages to be available for dequeuing. This is the default value.


Advanced Queuing: Enqueue Visibility Modes
------------------------------------------

These constants are extensions to the DB API definition. They are possible
values for the :attr:`~EnqOptions.visibility` attribute of the
:ref:`enqueue options object <enqoptions>`. This object is the options argument
for the :meth:`Connection.enq()` method.


.. data:: ENQ_IMMEDIATE

   This constant is used to specify that enqueue should perform its work as
   part of an independent transaction.


.. data:: ENQ_ON_COMMIT

   This constant is used to specify that enqueue should be part of the current
   transaction. This is the default value.


Advanced Queuing: Message States
--------------------------------

These constants are extensions to the DB API definition. They are possible
values for the :attr:`~MessageProperties.state` attribute of the
:ref:`message properties object <msgproperties>`. This object is the
msgproperties argument for the :meth:`Connection.deq()` and
:meth:`Connection.enq()` methods.


.. data:: MSG_EXPIRED

   This constant is used to specify that the message has been moved to the
   exception queue.


.. data:: MSG_PROCESSED

   This constant is used to specify that the message has been processed and
   has been retained.


.. data:: MSG_READY

   This constant is used to specify that the message is ready to be processed.


.. data:: MSG_WAITING

   This constant is used to specify that the message delay has not yet been
   reached.


Advanced Queuing: Other
-----------------------

These constants are extensions to the DB API definition. They are special
constants used in advanced queuing.


.. data:: MSG_NO_DELAY

   This constant is a possible value for the :attr:`~MessageProperties.delay`
   attribute of the :ref:`message properties object <msgproperties>` passed
   as the msgproperties parameter to the :meth:`Connection.deq()` and
   :meth:`Connection.enq()` methods. It specifies that no delay should be
   imposed and the message should be immediately available for dequeuing. This
   is also the default value.


.. data:: MSG_NO_EXPIRATION

   This constant is a possible value for the
   :attr:`~MessageProperties.expiration` attribute of the
   :ref:`message properties object <msgproperties>` passed as the msgproperties
   parameter to the :meth:`Connection.deq()` and :meth:`Connection.enq()`
   methods. It specifies that the message never expires. This is also the
   default value.


Connection Authorization Modes
------------------------------

These constants are extensions to the DB API definition. They are possible
values for the mode parameter of the :meth:`connect()` method.


.. data:: PRELIM_AUTH

   This constant is used to specify that preliminary authentication is to be
   used. This is needed for performing database startup and shutdown.


.. data:: SYSASM

   This constant is used to specify that SYSASM access is to be acquired.


.. data:: SYSDBA

   This constant is used to specify that SYSDBA access is to be acquired.


.. data:: SYSOPER

   This constant is used to specify that SYSOPER access is to be acquired.


Database Shutdown Modes
-----------------------

These constants are extensions to the DB API definition. They are possible
values for the mode parameter of the :meth:`Connection.shutdown()` method.


.. data:: DBSHUTDOWN_ABORT

   This constant is used to specify that the caller should not wait for current
   processing to complete or for users to disconnect from the database. This
   should only be used in unusual circumstances since database recovery may be
   necessary upon next startup.


.. data:: DBSHUTDOWN_FINAL

   This constant is used to specify that the instance can be truly halted. This
   should only be done after the database has been shutdown with one of the
   other modes (except abort) and the database has been closed and dismounted
   using the appropriate SQL commands.


.. data:: DBSHUTDOWN_IMMEDIATE

   This constant is used to specify that all uncommitted transactions should be
   rolled back and any connected users should be disconnected.


.. data:: DBSHUTDOWN_TRANSACTIONAL

   This constant is used to specify that further connections to the database
   should be prohibited and no new transactions should be allowed. It then
   waits for all active transactions to complete.


.. data:: DBSHUTDOWN_TRANSACTIONAL_LOCAL

   This constant is used to specify that further connections to the database
   should be prohibited and no new transactions should be allowed. It then
   waits for only local active transactions to complete.


Event Types
-----------

These constants are extensions to the DB API definition. They are possible
values for the :attr:`Message.type` attribute of the messages that are sent
for subscriptions created by the :meth:`Connection.subscribe()` method.


.. data:: EVENT_DEREG

   This constant is used to specify that the subscription has been
   deregistered and no further notifications will be sent.


.. data:: EVENT_NONE

   This constant is used to specify no information is available about the
   event.


.. data:: EVENT_OBJCHANGE

   This constant is used to specify that a database change has taken place on a
   table registered with the :meth:`Subscription.registerquery()` method.


.. data:: EVENT_QUERYCHANGE

   This constant is used to specify that the result set of a query registered
   with the :meth:`Subscription.registerquery()` method has been changed.


.. data:: EVENT_SHUTDOWN

   This constant is used to specify that the instance is in the process of
   being shut down.


.. data:: EVENT_SHUTDOWN_ANY

   This constant is used to specify that any instance (when running RAC) is in
   the process of being shut down.


.. data:: EVENT_STARTUP

   This constant is used to specify that the instance is in the process of
   being started up.


Operation Codes
---------------

These constants are extensions to the DB API definition. They are possible
values for the operations argument for the :meth:`Connection.subscribe()`
method. One or more of these values can be OR'ed together. These values are
also used by the :attr:`MessageTable.operation` or
:attr:`MessageQuery.operation` attributes of the messages that are sent.


.. data:: OPCODE_ALLOPS

   This constant is used to specify that messages should be sent for all
   operations.


.. data:: OPCODE_ALLROWS

   This constant is used to specify that the table or query has been completely
   invalidated.


.. data:: OPCODE_ALTER

   This constant is used to specify that messages should be sent when a
   registered table has been altered in some fashion by DDL, or that the
   message identifies a table that has been altered.


.. data:: OPCODE_DELETE

   This constant is used to specify that messages should be sent when data is
   deleted, or that the message identifies a row that has been deleted.


.. data:: OPCODE_DROP

   This constant is used to specify that messages should be sent when a
   registered table has been dropped, or that the message identifies a table
   that has been dropped.


.. data:: OPCODE_INSERT

   This constant is used to specify that messages should be sent when data is
   inserted, or that the message identifies a row that has been inserted.


.. data:: OPCODE_UPDATE

   This constant is used to specify that messages should be sent when data is
   updated, or that the message identifies a row that has been updated.


Session Pool Get Modes
----------------------

These constants are extensions to the DB API definition. They are possible
values for the getmode parameter of the :meth:`SessionPool()` method.


.. data:: SPOOL_ATTRVAL_FORCEGET

   This constant is used to specify that a new connection will be returned if
   there are no free sessions available in the pool.


.. data:: SPOOL_ATTRVAL_NOWAIT

   This constant is used to specify that an exception should be raised if there
   are no free sessions available in the pool. This is the default value.


.. data:: SPOOL_ATTRVAL_WAIT

   This constant is used to specify that the caller should wait until a session
   is available if there are no free sessions available in the pool.



Session Pool Purity
-------------------

These constants are extensions to the DB API definition. They are possible
values for the purity parameter of the :meth:`connect()` method, which is used
in database resident connection pooling (DRCP).


.. data:: ATTR_PURITY_DEFAULT

   This constant is used to specify that the purity of the session is the
   default value identified by Oracle (see Oracle's documentation for more
   information). This is the default value.


.. data:: ATTR_PURITY_NEW

   This constant is used to specify that the session acquired from the pool
   should be new and not have any prior session state.


.. data:: ATTR_PURITY_SELF

   This constant is used to specify that the session acquired from the pool
   need not be new and may have prior session state.


Subscription Namespaces
-----------------------

These constants are extensions to the DB API definition. They are possible
values for the namespace parameter of the :meth:`Connection.subscribe()`
method.


.. data:: SUBSCR_NAMESPACE_DBCHANGE

   This constant is used to specify that database change notification or query
   change notification messages are to be sent. This is the default value and
   currently the only value that is supported.


Subscription Protocols
----------------------

These constants are extensions to the DB API definition. They are possible
values for the protocol parameter of the :meth:`Connection.subscribe()` method.


.. data:: SUBSCR_PROTO_HTTP

   This constant is used to specify that notifications will be sent to an
   HTTP URL when a message is generated. This value is currently not supported.


.. data:: SUBSCR_PROTO_MAIL

   This constant is used to specify that notifications will be sent to an
   e-mail address when a message is generated. This value is currently not
   supported.


.. data:: SUBSCR_PROTO_OCI

   This constant is used to specify that notifications will be sent to the
   callback routine identified when the subscription was created. It is the
   default value and the only value currently supported.


.. data:: SUBSCR_PROTO_SERVER

   This constant is used to specify that notifications will be sent to a
   PL/SQL procedure when a message is generated. This value is currently not
   supported.


Subscription Quality of Service
-------------------------------

These constants are extensions to the DB API definition. They are possible
values for the qos parameter of the :meth:`Connection.subscribe()` method. One
or more of these values can be OR'ed together.

.. data:: SUBSCR_CQ_QOS_BEST_EFFORT

   This constant is used to specify that best effort filtering for query result
   set changes is acceptable. False positive notifications may be received.
   This behaviour may be suitable for caching applications.

   .. deprecated:: 5.3
      Use :data:`~cx_Oracle.SUBSCR_QOS_BEST_EFFORT` instead.


.. data:: SUBSCR_CQ_QOS_QUERY

   This constant is used to specify that notifications should be sent if the
   result set of the registered query changes. By default no false positive
   notifications will be generated.

   .. deprecated:: 5.3
      Use :data:`~cx_Oracle.SUBSCR_QOS_QUERY` instead.


.. data:: SUBSCR_QOS_BEST_EFFORT

   This constant is used to specify that best effort filtering for query result
   set changes is acceptable. False positive notifications may be received.
   This behaviour may be suitable for caching applications.


.. data:: SUBSCR_QOS_DEREG_NFY

   This constant is used to specify that the subscription should be
   automatically unregistered after the first notification is received.


.. data:: SUBSCR_QOS_PURGE_ON_NTFN

   This constant is used to specify that the subscription should be
   automatically unregistered after the first notification is received.

   .. deprecated:: 5.3
      Use :data:`~cx_Oracle.SUBSCR_QOS_DEREG_NFY` instead.


.. data:: SUBSCR_QOS_QUERY

   This constant is used to specify that notifications should be sent if the
   result set of the registered query changes. By default no false positive
   notifications will be generated.


.. data:: SUBSCR_QOS_RELIABLE

   This constant is used to specify that notifications should not be lost in
   the event of database failure.


.. data:: SUBSCR_QOS_ROWIDS

   This constant is used to specify that the rowids of the inserted, updated or
   deleted rows should be included in the message objects that are sent.


Types
=====

.. data:: BINARY

   This type object is used to describe columns in a database that contain
   binary data. In Oracle this is RAW columns.


.. data:: BFILE

   This type object is used to describe columns in a database that are BFILEs.

   .. note::

      This type is an extension to the DB API definition.


.. data:: BLOB

   This type object is used to describe columns in a database that are BLOBs.

   .. note::

      This type is an extension to the DB API definition.


.. data:: BOOLEAN

   This type object is used to represent PL/SQL booleans.

   .. versionadded:: 5.2.1

   .. note::

      This type is an extension to the DB API definition. It is only available
      in Oracle 12.1 and higher and only within PL/SQL. It cannot be used in
      columns.


.. data:: CLOB

   This type object is used to describe columns in a database that are CLOBs.

   .. note::

      This type is an extension to the DB API definition.


.. data:: CURSOR

   This type object is used to describe columns in a database that are cursors
   (in PL/SQL these are known as ref cursors).

   .. note::

      This type is an extension to the DB API definition.


.. data:: DATETIME

   This type object is used to describe columns in a database that are dates.


.. data:: FIXED_CHAR

   This type object is used to describe columns in a database that are fixed
   length strings (in Oracle these is CHAR columns); these behave differently
   in Oracle than varchar2 so they are differentiated here even though the DB
   API does not differentiate them.

   .. note::

      This attribute is an extension to the DB API definition.


.. data:: FIXED_NCHAR

   This type object is used to describe columns in a database that are NCHAR
   columns in Oracle; these behave differently in Oracle than nvarchar2 so they
   are differentiated here even though the DB API does not differentiate them.

   .. note::

      This type is an extension to the DB API definition.


.. data:: INTERVAL

   This type object is used to describe columns in a database that are of type
   interval day to second.

   .. note::

      This type is an extension to the DB API definition.


.. data:: LOB

   This type object is the Python type of :data:`BLOB` and :data:`CLOB` data
   that is returned from cursors.

   .. note::

      This type is an extension to the DB API definition.


.. data:: LONG_BINARY

   This type object is used to describe columns in a database that are long
   binary (in Oracle these are LONG RAW columns).

   .. note::

      This type is an extension to the DB API definition.


.. data:: LONG_NCHAR

   This type object is used to describe columns in a database that are long
   NCHAR columns.  There is no direct support for this in Oracle but long
   NCHAR strings are bound this way in order to avoid the "unimplemented or
   unreasonable conversion requested" error.

   .. deprecated:: 5.3

   .. note::

      This type is an extension to the DB API definition.


.. data:: LONG_STRING

   This type object is used to describe columns in a database that are long
   strings (in Oracle these are LONG columns).

   .. note::

      This type is an extension to the DB API definition.


.. data:: NATIVE_FLOAT

   This type object is used to describe columns in a database that are of type
   binary_double or binary_float.

   .. note::

      This type is an extension to the DB API definition.


.. data:: NATIVE_INT

   This type object is used to bind integers using Oracle's native integer
   support, rather than the standard number support, which improves
   performance.

   .. versionadded:: 5.3

   .. note::

      This type is an extension to the DB API definition.


.. data:: NCHAR

   This type object is used to describe national character strings (NVARCHAR2)
   in Oracle.

   .. note::

      This type is an extension to the DB API definition.


.. data:: NCLOB

   This type object is used to describe columns in a database that are NCLOBs.

   .. note::

      This type is an extension to the DB API definition.


.. data:: NUMBER

   This type object is used to describe columns in a database that are numbers.


.. data:: OBJECT

   This type object is used to describe columns in a database that are objects.

   .. note::

      This type is an extension to the DB API definition.


.. data:: ROWID

   This type object is used to describe the pseudo column "rowid".


.. data:: STRING

   This type object is used to describe columns in a database that are strings
   (in Oracle this is VARCHAR2 columns).


.. data:: TIMESTAMP

   This type object is used to describe columns in a database that are
   timestamps.

   .. note::

      This attribute is an extension to the DB API definition.


.. _exceptions:

Exceptions
==========

.. exception:: Warning

   Exception raised for important warnings and defined by the DB API but not
   actually used by cx_Oracle.


.. exception:: Error

   Exception that is the base class of all other exceptions defined by
   cx_Oracle and is a subclass of the Python StandardError exception (defined
   in the module exceptions).


.. exception:: InterfaceError

   Exception raised for errors that are related to the database interface
   rather than the database itself. It is a subclass of Error.


.. exception:: DatabaseError

   Exception raised for errors that are related to the database. It is a
   subclass of Error.


.. exception:: DataError

   Exception raised for errors that are due to problems with the processed
   data. It is a subclass of DatabaseError.


.. exception:: OperationalError

   Exception raised for errors that are related to the operation of the
   database but are not necessarily under the control of the progammer. It is a
   subclass of DatabaseError.


.. exception:: IntegrityError

   Exception raised when the relational integrity of the database is affected.
   It is a subclass of DatabaseError.


.. exception:: InternalError

   Exception raised when the database encounters an internal error. It is a
   subclass of DatabaseError.


.. exception:: ProgrammingError

   Exception raised for programming errors. It is a subclass of DatabaseError.


.. exception:: NotSupportedError

   Exception raised when a method or database API was used which is not
   supported by the database. It is a subclass of DatabaseError.


Exception handling
==================

.. note::

   PEP 249 (Python Database API Specification v2.0) says the following about
   exception values:

       [...] The values of these exceptions are not defined. They should
       give the user a fairly good idea of what went wrong, though. [...]

   With cx_Oracle every exception object has exactly one argument in the
   ``args`` tuple. This argument is a ``cx_Oracle._Error`` object which has
   the following five read-only attributes.

.. attribute:: _Error.code

   Integer attribute representing the Oracle error number (ORA-XXXXX).

.. attribute:: _Error.offset

   Integer attribute representing the error offset when applicable.

.. attribute:: _Error.message

   String attribute representing the Oracle message of the error. This
   message is localized by the environment of the Oracle connection.

.. attribute:: _Error.context

   String attribute representing the context in which the exception was
   raised.

.. attribute:: _Error.isrecoverable

   Boolean attribute representing whether the error is recoverable or not. This
   is False in all cases unless Oracle Database 12.1 is being used on both the
   server and the client.

   .. versionadded:: 5.3


This allows you to use the exceptions for example in the following way:

::

    from __future__ import print_function

    import cx_Oracle

    connection = cx_Oracle.Connection("cx_Oracle/dev@localhost/orcl")
    cursor = connection.cursor()

    try:
        cursor.execute("select 1 / 0 from dual")
    except cx_Oracle.DatabaseError as exc:
        error, = exc.args
        print("Oracle-Error-Code:", error.code)
        print("Oracle-Error-Message:", error.message)

