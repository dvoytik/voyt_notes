New bugs
--------


KFENCE
KASAN
KMSAN
KCSAN
UBSAN (CONFIG_UBSAN_ALIGNMENT - disable on x86 because CONFIG_HAVE_EFFICIENT_UNALIGNED_ACCESS=y)
CONFIG_DMA_API_DEBUG
LockdepSleeping

CONFIG_IOMMU_LEAK
	  Add a simple leak tracer to the IOMMU code. This is useful when you
	  are debugging a buggy device driver that leaks IOMMU mappings.
CONFIG_DMA_API_DEBUG=y
	  Enable this option to debug the use of the DMA API by device drivers.
	  With this option you will be able to detect common bugs in device
	  drivers like double-freeing of DMA mappings or freeing mappings that
	  were never allocated.

	  This option causes a performance degradation.  Use only if you want to
	  debug device drivers and dma interactions.

	  If unsure, say N.

config X86_DECODER_SELFTEST
	  Perform x86 instruction decoder selftests at build time.
	  This option is useful for checking the sanity of x86 instruction
	  decoder code.
	  If unsure, say "N".

config DYNAMIC_DEBUG
	bool "Enable dynamic printk() support"
	default n
	depends on PRINTK
	depends on (DEBUG_FS || PROC_FS)
	select DYNAMIC_DEBUG_CORE
	help

	  Compiles debug level messages into the kernel, which would not
	  otherwise be available at runtime. These messages can then be
	  enabled/disabled based on various levels of scope - per source file,
	  function, module, format string, and line number. This mechanism
	  implicitly compiles in all pr_debug() and dev_dbg() calls, which
	  enlarges the kernel text size by about 2%.

	  If a source file is compiled with DEBUG flag set, any
	  pr_debug() calls in it are enabled by default, but can be
	  disabled at runtime as below.  Note that DEBUG flag is
	  turned on by many CONFIG_*DEBUG* options.

	  Usage:

	  Dynamic debugging is controlled via the 'dynamic_debug/control' file,
	  which is contained in the 'debugfs' filesystem or procfs.
	  Thus, the debugfs or procfs filesystem must first be mounted before
	  making use of this feature.
	  We refer the control file as: <debugfs>/dynamic_debug/control. This
	  file contains a list of the debug statements that can be enabled. The
	  format for each line of the file is:

		filename:lineno [module]function flags format

	  filename : source file of the debug statement
	  lineno : line number of the debug statement
	  module : module that contains the debug statement
	  function : function that contains the debug statement
	  flags : '=p' means the line is turned 'on' for printing
	  format : the format used for the debug statement

	  From a live system:

		nullarbor:~ # cat <debugfs>/dynamic_debug/control
		# filename:lineno [module]function flags format
		fs/aio.c:222 [aio]__put_ioctx =_ "__put_ioctx:\040freeing\040%p\012"
		fs/aio.c:248 [aio]ioctx_alloc =_ "ENOMEM:\040nr_events\040too\040high\012"
		fs/aio.c:1770 [aio]sys_io_cancel =_ "calling\040cancel\012"

	  Example usage:

		// enable the message at line 1603 of file svcsock.c
		nullarbor:~ # echo -n 'file svcsock.c line 1603 +p' >
						<debugfs>/dynamic_debug/control

		// enable all the messages in file svcsock.c
		nullarbor:~ # echo -n 'file svcsock.c +p' >
						<debugfs>/dynamic_debug/control

		// enable all the messages in the NFS server module
		nullarbor:~ # echo -n 'module nfsd +p' >
						<debugfs>/dynamic_debug/control

		// enable all 12 messages in the function svc_process()
		nullarbor:~ # echo -n 'func svc_process +p' >
						<debugfs>/dynamic_debug/control

		// disable all 12 messages in the function svc_process()
		nullarbor:~ # echo -n 'func svc_process -p' >
						<debugfs>/dynamic_debug/control

	  See Documentation/admin-guide/dynamic-debug-howto.rst for additional
	  information.


config GDB_SCRIPTS
	bool "Provide GDB scripts for kernel debugging"
	help
	  This creates the required links to GDB helper scripts in the
	  build directory. If you load vmlinux into gdb, the helper
	  scripts will be automatically imported by gdb as well, and
	  additional functions are available to analyze a Linux kernel
	  instance. See Documentation/process/debugging/gdb-kernel-debugging.rst
	  for further details.


config READABLE_ASM
	bool "Generate readable assembler code"
	depends on DEBUG_KERNEL
	depends on CC_IS_GCC
	help
	  Disable some compiler optimizations that tend to generate human unreadable
	  assembler output. This may make the kernel slightly slower, but it helps
	  to keep kernel developers who have to stare a lot at assembler listings
	  sane.

config HEADERS_INSTALL
	bool "Install uapi headers to usr/include"
	help
	  This option will install uapi headers (headers exported to user-space)
	  into the usr/include directory for use during the kernel build.
	  This is unneeded for building the kernel itself, but needed for some
	  user-space program samples. It is also needed by some features such
	  as uapi header sanity checks.

config OBJTOOL_WERROR
	bool "Upgrade objtool warnings to errors"
	depends on OBJTOOL && !COMPILE_TEST
	help
	  Fail the build on objtool warnings.

	  Objtool warnings can indicate kernel instability, including boot
	  failures.  This option is highly recommended.

	  If unsure, say Y.

config WARN_CONTEXT_ANALYSIS
	bool "Compiler context-analysis warnings"
	depends on CC_IS_CLANG && CLANG_VERSION >= 230000
	# Branch profiling re-defines "if", which messes with the compiler's
	# ability to analyze __cond_acquires(..), resulting in false positives.
	depends on !TRACE_BRANCH_PROFILING
	default y
	help
	  Context Analysis is a language extension, which enables statically
	  checking that required contexts are active (or inactive) by acquiring
	  and releasing user-definable "context locks".

	  Clang's name of the feature is "Thread Safety Analysis". Requires
	  Clang 23 or later.

	  Produces warnings by default. Select CONFIG_WERROR if you wish to
	  turn these warnings into errors.

	  For more details, see Documentation/dev-tools/context-analysis.rst.

config WARN_CONTEXT_ANALYSIS_ALL
	bool "Enable context analysis for all source files"
	depends on WARN_CONTEXT_ANALYSIS
	depends on EXPERT && !COMPILE_TEST
	help
	  Enable tree-wide context analysis. This is likely to produce a
	  large number of false positives - enable at your own risk.

	  If unsure, say N.


config DEBUG_OBJECTS
	bool "Debug object operations"
	depends on PREEMPT_COUNT || !DEFERRED_STRUCT_PAGE_INIT
	depends on DEBUG_KERNEL
	help
	  If you say Y here, additional code will be inserted into the
	  kernel to track the life time of various objects and validate
	  the operations on those objects.

menuconfig KGDB
	bool "KGDB: kernel debugger"
	depends on HAVE_ARCH_KGDB
	depends on DEBUG_KERNEL
	help
	  If you say Y here, it will be possible to remotely debug the
	  kernel using gdb.  It is recommended but not required, that
	  you also turn on the kernel config option
	  CONFIG_FRAME_POINTER to aid in producing more reliable stack
	  backtraces in the external debugger.  Documentation of
	  kernel debugger is available at http://kgdb.sourceforge.net
	  as well as in Documentation/process/debugging/kgdb.rst.  If
	  unsure, say N.

menuconfig UBSAN
	bool "Undefined behaviour sanity checker"
	depends on ARCH_HAS_UBSAN
	help
	  This option enables the Undefined Behaviour sanity checker.
	  Compile-time instrumentation is used to detect various undefined
	  behaviours at runtime. For more details, see:
	  Documentation/dev-tools/ubsan.rst
