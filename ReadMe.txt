WIP Modified version of IDA ClassInformer

original readme:
================

Class Informer:
=========================================================
IDA Pro class vftable finder, namer, fixer, lister plug-in.
Version 2.0, May 2015
By Sirmabus

https://sourceforge.net/projects/classinformer/
http://www.macromonkey.com/bb/index.php/topic,13.0.html

See Plugin\Class_Informer.txt for more..

end of original readme.
=======================

Changes to MSVC vftables in Class Informer Modified:
====================================================
	Compiled with the 6.9 SDK.
	Implemented virtual functions naming (as ClassName::FuncXXXX) (Name propagated through comments when needed)
		Note: manually naming a parent or a child virtual function should propagate on next run of the plugin.
		Functions used in multiple unrelated classes are not renamed.

	Virtual function table discovery split in two passes so detailed discovery can be done in "inheritance order"
	Non RTTI classes referenced in inheritance or template are created as empty struct.
	Templates are tentatively processed.
	All invalid characters in classes names are replaced by underscore when used in IDA names.

	Known issue:
		Anything related to templates is very bad and unfinished at the moment.
		Multiple naming convention and logic may have left useless code :)
		The automated process is far from perfect so review should be done (and use a copy of your database :)).
		Classes whose name is too long are "renamed" arbitrarily and can interrupt the process while renaming functions.
			Just ignore the duplicate name warning. 

	TBD:
		Using relative offsets in exported data rather than absolute to ease comparisons between patches.

	Attempt at :
		Discovering use of the vfTable address in constructor (and destructor).
			Inlined constructor discovery is poor and almost need case by case tailoring.
		Using discovered constructors usage to calculate classes size.
		Using classes size to prototype classes as local type struct with inheritance.

	Exported data:
		All exported files goes into a subdirectory in the same directory as the IDA database and using the same name.
		CSV exports: Classes, Class hierachy and Class virtual members at the moment.
		C header file dumping the known classes struct. 
			Purpose is to iterate:
				Run IDA_ClassInformer_Modified and export
				Edit manually to a new file.
				Import (all or a part of the new file) and run IDA_ClassInformer_Modified to propagate.
		C++ header file dumping the known classes and virtual functions.
			Purpose is to copy and paste into a related project.
		Include file dumping known information about constructor, destructor and classes size.
		List of RTTI Type Descriptor (??_R0?AV...) for DynamicCast
		And currently useless text file used as a scratch pad.
