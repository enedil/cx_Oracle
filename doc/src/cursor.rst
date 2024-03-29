.. _cursorobj:

*************
Cursor Object
*************


.. attribute:: Cursor.arraysize

   This read-write attribute specifies the number of rows to fetch at a time
   internally and is the default number of rows to fetch with the
   :meth:`~Cursor.fetchmany()` call.  It defaults to 100 meaning to fetch 100
   rows at a time. Note that this attribute can drastically affect the
   performance of a query since it directly affects the number of network round
   trips that need to be performed. This is the reason for setting it to 100
   instead of the 1 that the DB API recommends.


.. attribute:: Cursor.bindarraysize

   This read-write attribute specifies the number of rows to bind at a time and
   is used when creating variables via :meth:`~Cursor.setinputsizes()` or
   :meth:`~Cursor.var()`. It defaults to 1 meaning to bind a single row at a
   time.

   .. note::

      The DB API definition does not define this attribute.


.. method:: Cursor.arrayvar(dataType, value, [size])

   Create an array variable associated with the cursor of the given type and
   size and return a :ref:`variable object <varobj>`. The value is either an
   integer specifying the number of elements to allocate or it is a list and
   the number of elements allocated is drawn from the size of the list. If the
   value is a list, the variable is also set with the contents of the list. If
   the size is not specified and the type is a string or binary, 4000 bytes
   is allocated. This is needed for passing arrays to PL/SQL (in cases where
   the list might be empty and the type cannot be determined automatically) or
   returning arrays from PL/SQL.

   .. note::

      The DB API definition does not define this method.


.. method:: Cursor.bindnames()

   Return the list of bind variable names bound to the statement. Note that a
   statement must have been prepared first.

   .. note::

      The DB API definition does not define this method.


.. attribute:: Cursor.bindvars

   This read-only attribute provides the bind variables used for the last
   execute. The value will be either a list or a dictionary depending on
   whether binding was done by position or name. Care should be taken when
   referencing this attribute. In particular, elements should not be removed.

   .. note::

      The DB API definition does not define this attribute.


.. method:: Cursor.callfunc(name, returnType, parameters=[], keywordParameters={})

   Call a function with the given name. The return type is specified in the
   same notation as is required by :meth:`~Cursor.setinputsizes()`. The
   sequence of parameters must contain one entry for each argument that the
   function expects. Any keyword parameters will be included after the
   positional parameters. The result of the call is the return value of the
   function.

   .. note::

      The DB API definition does not define this method.

   .. note::

      If you intend to call :meth:`Cursor.setinputsizes()` on the cursor prior
      to making this call, then note that the first item in the argument list
      refers to the return value of the function.


.. method:: Cursor.callproc(name, parameters=[], keywordParameters={})

   Call a procedure with the given name. The sequence of parameters must
   contain one entry for each argument that the procedure expects. The result
   of the call is a modified copy of the input sequence. Input parameters are
   left untouched; output and input/output parameters are replaced with
   possibly new values. Keyword parameters will be included after the
   positional parameters and are not returned as part of the output sequence.

   .. note::

      The DB API definition does not allow for keyword parameters.


.. method:: Cursor.close()

   Close the cursor now, rather than whenever __del__ is called. The cursor
   will be unusable from this point forward; an Error exception will be raised
   if any operation is attempted with the cursor.


.. attribute:: Cursor.connection

   This read-only attribute returns a reference to the connection object on
   which the cursor was created.

   .. note::

      This attribute is an extension to the DB API definition but it is
      mentioned in PEP 249 as an optional extension.


.. data:: Cursor.description

   This read-only attribute is a sequence of 7-item sequences. Each of these
   sequences contains information describing one result column: (name, type,
   display_size, internal_size, precision, scale, null_ok). This attribute will
   be None for operations that do not return rows or if the cursor has not had
   an operation invoked via the :meth:`~Cursor.execute()` method yet.

   The type will be one of the type objects defined at the module level.


.. method:: Cursor.execute(statement, [parameters], \*\*keywordParameters)

   Execute a statement against the database. Parameters may be passed as a
   dictionary or sequence or as keyword arguments. If the arguments are a
   dictionary, the values will be bound by name and if the arguments are a
   sequence the values will be bound by position. Note that if the values are
   bound by position, the order of the variables is from left to right as they
   are encountered in the statement.

   A reference to the statement will be retained by the cursor. If None or the
   same string object is passed in again, the cursor will execute that
   statement again without performing a prepare or rebinding and redefining.
   This is most effective for algorithms where the same statement is used, but
   different parameters are bound to it (many times). Note that parameters that
   are not passed in during subsequent executions will retain the value passed
   in during the last execution that contained them.

   For maximum efficiency when reusing an statement, it is best to use the
   :meth:`~Cursor.setinputsizes()` method to specify the parameter types and
   sizes ahead of time; in particular, None is assumed to be a string of length
   1 so any values that are later bound as numbers or dates will raise a
   TypeError exception.

   If the statement is a query, the cursor is returned as a convenience to the
   caller (so it can be used directly as an iterator over the rows in the
   cursor); otherwise, ``None`` is returned.

   .. note

      ::The DB API definition does not define the return value of this method.


.. method:: Cursor.executemany(statement, parameters, batcherrors=False, arraydmlrowcounts=False)

   Prepare a statement for execution against a database and then execute it
   against all parameter mappings or sequences found in the sequence
   parameters. The statement is managed in the same way as the
   :meth:`~Cursor.execute()` method manages it.

   When true, the batcherrors parameter enables batch error support within
   Oracle and ensures that the call succeeds even if an exception takes place
   in one or more of the sequence of parameters. The errors can then be
   retrieved using :meth:`~Cursor.getbatcherrors()`.

   When true, the arraydmlrowcounts parameter enables DML row counts to be
   retrieved from Oracle after the method has completed. The row counts can
   then be retrieved using :meth:`~Cursor.getarraydmlrowcounts()`.


.. method:: Cursor.executemanyprepared(numIters)

   Execute the previously prepared and bound statement the given number of
   times.  The variables that are bound must have already been set to their
   desired value before this call is made.  This method was designed for the
   case where optimal performance is required as it comes at the expense of
   compatibility with the DB API.

   .. note::

      The DB API definition does not define this method.


.. method:: Cursor.fetchall()

   Fetch all (remaining) rows of a query result, returning them as a list of
   tuples. An empty list is returned if no more rows are available. Note that
   the cursor's arraysize attribute can affect the performance of this
   operation, as internally reads from the database are done in batches
   corresponding to the arraysize.

   An exception is raised if the previous call to :meth:`~Cursor.execute()` did
   not produce any result set or no call was issued yet.


.. method:: Cursor.fetchmany([numRows=cursor.arraysize])

   Fetch the next set of rows of a query result, returning a list of tuples. An
   empty list is returned if no more rows are available. Note that the cursor's
   arraysize attribute can affect the performance of this operation.

   The number of rows to fetch is specified by the parameter. If it is not
   given, the cursor's arrysize attribute determines the number of rows to be
   fetched. If the number of rows available to be fetched is fewer than the
   amount requested, fewer rows will be returned.

   An exception is raised if the previous call to :meth:`~Cursor.execute()` did
   not produce any result set or no call was issued yet.


.. method:: Cursor.fetchone()

   Fetch the next row of a query result set, returning a single tuple or None
   when no more data is available.

   An exception is raised if the previous call to :meth:`~Cursor.execute()` did
   not produce any result set or no call was issued yet.


.. method:: Cursor.fetchraw([numRows=cursor.arraysize])

   Fetch the next set of rows of a query result into the internal buffers of
   the defined variables for the cursor. The number of rows actually fetched is
   returned.  This method was designed for the case where optimal performance
   is required as it comes at the expense of compatibility with the DB API.

   An exception is raised if the previous call to :meth:`~Cursor.execute()` did
   not produce any result set or no call was issued yet.

   .. note::

      The DB API definition does not define this method.


.. attribute:: Cursor.fetchvars

   This read-only attribute specifies the list of variables created for the
   last query that was executed on the cursor.  Care should be taken when
   referencing this attribute. In particular, elements should not be removed.

   .. note::

      The DB API definition does not define this attribute.


.. method:: Cursor.getarraydmlrowcounts()

   Retrieve the DML row counts after a call to :meth:`~Cursor.executemany()`
   with arraydmlrowcounts enabled. This will return a list of integers
   corresponding to the number of rows affected by the DML statement for each
   element of the array passed to :meth:`~Cursor.executemany()`.

   .. note::

      The DB API definition does not define this method and it is only
      available for Oracle 12.1 and higher.


.. method:: Cursor.getbatcherrors()

   Retrieve the exceptions that took place after a call to
   :meth:`~Cursor.executemany()` with batcherors enabled. This will return a
   list of Error objects, one error for each iteration that failed. The offset
   can be determined by looking at the offset attribute of the error object.

   .. note::

      The DB API definition does not define this method.


.. method:: Cursor.getimplicitresults()

   Return a list of cursors which correspond to implicit results made available
   from a PL/SQL block or procedure without the use of OUT ref cursor
   parameters. The PL/SQL block or procedure opens the cursors and marks them 
   for return to the client using the procedure dbms_sql.return_result. Cursors
   returned in this fashion should not be closed. They will be closed
   automatically by the parent cursor when it is closed. Closing the parent
   cursor will invalidate the cursors returned by this method.

   .. versionadded:: 5.3

   .. note::

      The DB API definition does not define this method and it is only
      available for Oracle Database 12.1 (both client and server must be at
      this level or higher). It is most like the DB API method nextset(), but
      unlike that method (which requires that the next result set overwrite
      the current result set), this method returns cursors which can be fetched
      independently of each other.


.. attribute:: Cursor.inputtypehandler

   This read-write attribute specifies a method called for each value that is
   bound to a statement executed on the cursor and overrides the attribute with
   the same name on the connection if specified. The method signature is
   handler(cursor, value, arraysize) and the return value is expected to be a
   variable object or None in which case a default variable object will be
   created. If this attribute is None, the value of the attribute with the same
   name on the connection is used.

   .. note::

      This attribute is an extension to the DB API definition.


.. method:: Cursor.__iter__()

   Returns the cursor itself to be used as an iterator.

   .. note::

      This method is an extension to the DB API definition but it is
      mentioned in PEP 249 as an optional extension.


.. method:: Cursor.next()

   Fetch the next row of a query result set, using the same semantics as the
   method fetchone().

   .. note::

      This method is an extension to the DB API definition but it is
      mentioned in PEP 249 as an optional extension.


.. attribute:: Cursor.numbersAsStrings

   This integer attribute defines whether or not numbers should be returned as
   strings rather than integers or floating point numbers. This is useful to
   get around the fact that Oracle floating point numbers have considerably
   greater precision than C floating point numbers and not require a change to
   the SQL being executed.

   .. deprecated:: 5.3

      Use one of the attributes :attr:`Connection.outputtypehandler` or
      :attr:`~Cursor.outputtypehandler` instead.

   .. note::

      The DB API definition does not define this attribute.


.. attribute:: Cursor.outputtypehandler

   This read-write attribute specifies a method called for each column that is
   to be fetched from this cursor. The method signature is
   handler(cursor, name, defaultType, length, precision, scale) and the return
   value is expected to be a variable object or None in which case a default
   variable object will be created. If this attribute is None, the value of
   the attribute with the same name on the connection is used instead.

   .. note::

      This attribute is an extension to the DB API definition.


.. method:: Cursor.parse(statement)

   This can be used to parse a statement without actually executing it (this
   step is done automatically by Oracle when a statement is executed).

   .. note::

      The DB API definition does not define this method.

   .. note::

      You can parse any DML or DDL statement. DDL statements are executed
      immediately and an implied commit takes place.


.. method:: Cursor.prepare(statement, [tag])

   This can be used before a call to :meth:`~Cursor.execute()` to define the
   statement that will be executed. When this is done, the prepare phase will
   not be performed when the call to :meth:`~Cursor.execute()` is made with
   None or the same string object as the statement.  If specified the statement
   will be returned to the statement cache with the given tag. See the Oracle
   documentation for more information about the statement cache.

   .. note::

      The DB API definition does not define this method.


.. attribute:: Cursor.rowcount

   This read-only attribute specifies the number of rows that have currently
   been fetched from the cursor (for select statements) or that have been
   affected by the operation (for insert, update and delete statements).


.. attribute:: Cursor.rowfactory

   This read-write attribute specifies a method to call for each row that is
   retrieved from the database. Ordinarily a tuple is returned for each row but
   if this attribute is set, the method is called with the argument tuple that
   would normally be returned and the result of the method is returned instead.

   .. note::

      The DB API definition does not define this attribute.


.. method:: Cursor.scroll(value=0, mode="relative")

   Scroll the cursor in the result set to a new position according to the mode.

   If mode is "relative" (the default value), the value is taken as an offset
   to the current position in the result set. If set to "absolute", value
   states an absolute target position. If set to "first", the cursor is
   positioned at the first row and if set to "last", the cursor is set to the
   last row in the result set.

   An error is raised if the mode is "relative" or "absolute" and the scroll
   operation would position the cursor outside of the result set.

   .. versionadded:: 5.3

   .. note::

      This method is an extension to the DB API definition but it is
      mentioned in PEP 249 as an optional extension.


.. attribute:: Cursor.scrollable

   This read-write boolean attribute specifies whether the cursor can be
   scrolled or not. By default, cursors are not scrollable, as the server
   resources and response times are greater than nonscrollable cursors. This
   attribute is checked and the corresponding mode set in Oracle when calling
   the method :meth:`~Cursor.execute()`.

   .. versionadded:: 5.3

   .. note::

      The DB API definition does not define this attribute.


.. method:: Cursor.setinputsizes(\*args, \*\*keywordArgs)

   This can be used before a call to :meth:`~Cursor.execute()`,
   :meth:`~Cursor.callfunc()` or :meth:`~Cursor.callproc()` to predefine memory
   areas for the operation's parameters. Each parameter should be a type object
   corresponding to the input that will be used or it should be an integer
   specifying the maximum length of a string parameter. Use keyword arguments
   when binding by name and positional arguments when binding by position. The
   singleton None can be used as a parameter when using positional arguments to
   indicate that no space should be reserved for that position.

   .. note::

      If you plan to use :meth:`~Cursor.callfunc()` then be aware that the
      first argument in the list refers to the return value of the function.


.. method:: Cursor.setoutputsize(size, [column])

   This can be used before a call to :meth:`~Cursor.execute()` to predefine
   memory areas for the long columns that will be fetched. The column is
   specified as an index into the result sequence. Not specifying the column
   will set the default size for all large columns in the cursor.


.. attribute:: Cursor.statement

   This read-only attribute provides the string object that was previously
   prepared with :meth:`~Cursor.prepare()` or executed with
   :meth:`~Cursor.execute()`.

   .. note::

      The DB API definition does not define this attribute.


.. method:: Cursor.var(dataType, [size, arraysize, inconverter, outconverter, typename])

   Create a variable associated with the cursor of the given type and
   characteristics and return a :ref:`variable object <varobj>`. If the size is
   not specified and the type is a string or binary, 4000 bytes is allocated;
   if the size is not specified and the type is a long string or long binary,
   128KB is allocated. If the arraysize is not specified, the bind array size
   (usually 1) is used. The inconverter and outconverter specify methods used
   for converting values to/from the database. More information can be found in
   the section on variable objects.

   To create an empty SQL object variable, specify the typename parameter.

   This method was designed for use with PL/SQL in/out variables where the
   length or type cannot be determined automatically from the Python object
   passed in or for use in input and output type handlers defined on cursors
   or connections.

   .. note::

      The DB API definition does not define this method.

