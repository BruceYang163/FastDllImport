# FastDllImport
High performance DllImport mechanism to invoke unmanaged dll from C# managed code.

# Introduction
"DllImportAttribute", also known as PInvoke mechanism in .net framework, is used to call functions exported from native dll.
However, PInvoke procedure executes a lot of additional tasks from .net managed code to unmanged Dll code, when calling Dll with high frequency, PInvoke mechanism causes low performance.

"FastDllImportManager" is used to solve the problem, it can provide high efficiency function-call from .net to Dll. The performance is almost approaching to call C# built-in functions.

# How to use
We provide 2 options to use "`FastDllImportManager`":<br>
### [Option1]
1. Add "`FastDllImportManager`" class to the project<br>
2. Add an `[FastDllImportRedirected]` attribute on the `[DllImport]` function, such as:<br>
```c#
public partial class Form1 : Form
{
...
  [FastDllImportRedirected] 
  [DllImport("MyDll.dll")]
  public static extern int MyAdd1(int a);
...
}
```
3. Add a static construction function to call FastDllImportManager.Instance.Register(typeof(XXX)) for initialization, such as:<br>
(XXX is the class which use declare to import the DLL)<br>
```c#
static Form1()
{
  FastDllImportManager.Instance.Register(typeof(Form1));
}
```
### [Option2]
1. Same as Option1 above<br>
2. Use the following declaration:<br>
```c#
[FastDllImport("MyDll.dll")]
public delegate int F1(int arg1); static F1 MyAdd1;
```
instead of PInvoke declaration:<br>
```c#
[DllImport("MyDll.dll")]
public static extern int MyAdd1(int a);
```
3. Same as Option1 above.<br>

## Now, the Dll call will be accelerated.<br>
# Test result
##### Increase 2*100,000,000 times:
![](https://github.com/BruceYang163/FastDllImport/blob/master/test1.PNG)

# To be improved
1. Do not automatically support array parameter, if want to use array parameter, please use "`fixed`" key word to fix the array, like this:
```c#
fixed(byte* p = (byte*)&arr[0])
{
  DllFucn(p)
}
```
2. For Option2, only support 0/1/2/3 parameters in Dll functions, more parameters will be supported in the future.

