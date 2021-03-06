---
title: Find point on curve at distance
description: Demonstrates how find a point on a curve given a specified length from the start of the curve.
authors: ['steve_baer']
sdk: ['RhinoCommon']
languages: ['C#', 'Python', 'VB']
platforms: ['Windows', 'Mac']
categories: ['Curves']
origin: http://wiki.mcneel.com/developer/rhinocommonsamples/arclengthpoint
order: 1
keywords: ['find', 'point', 'curve', 'distance']
layout: code-sample-rhinocommon
---

```cs
partial class Examples
{
  public static Rhino.Commands.Result ArcLengthPoint(Rhino.RhinoDoc doc)
  {
    Rhino.DocObjects.ObjRef objref;
    Rhino.Commands.Result rc = Rhino.Input.RhinoGet.GetOneObject("Select curve",
      true, Rhino.DocObjects.ObjectType.Curve,out objref);
    if(rc!= Rhino.Commands.Result.Success)
      return rc;
    Rhino.Geometry.Curve crv = objref.Curve();
    if( crv==null )
      return Rhino.Commands.Result.Failure;

    double crv_length = crv.GetLength();
    double length = 0;
    rc = Rhino.Input.RhinoGet.GetNumber("Length from start", true, ref length, 0, crv_length);
    if(rc!= Rhino.Commands.Result.Success)
      return rc;

    Rhino.Geometry.Point3d pt = crv.PointAtLength(length);
    if (pt.IsValid)
    {
      doc.Objects.AddPoint(pt);
      doc.Views.Redraw();
    }
    return Rhino.Commands.Result.Success;
  }
}
```
{: #cs .tab-pane .fade .in .active}


```vbnet
Partial Friend Class Examples
  Public Shared Function ArcLengthPoint(ByVal doc As Rhino.RhinoDoc) As Rhino.Commands.Result
	Dim objref As Rhino.DocObjects.ObjRef = Nothing
	Dim rc As Rhino.Commands.Result = Rhino.Input.RhinoGet.GetOneObject("Select curve", True, Rhino.DocObjects.ObjectType.Curve,objref)
	If rc IsNot Rhino.Commands.Result.Success Then
	  Return rc
	End If
	Dim crv As Rhino.Geometry.Curve = objref.Curve()
	If crv Is Nothing Then
	  Return Rhino.Commands.Result.Failure
	End If

	Dim crv_length As Double = crv.GetLength()
	Dim length As Double = 0
	rc = Rhino.Input.RhinoGet.GetNumber("Length from start", True, length, 0, crv_length)
	If rc IsNot Rhino.Commands.Result.Success Then
	  Return rc
	End If

	Dim pt As Rhino.Geometry.Point3d = crv.PointAtLength(length)
	If pt.IsValid Then
	  doc.Objects.AddPoint(pt)
	  doc.Views.Redraw()
	End If
	Return Rhino.Commands.Result.Success
  End Function
End Class
```
{: #vb .tab-pane .fade .in}


```python
import Rhino
import scriptcontext

def ArcLengthPoint():
    rc, objref = Rhino.Input.RhinoGet.GetOneObject("Select curve", True, Rhino.DocObjects.ObjectType.Curve)
    if rc!=Rhino.Commands.Result.Success: return rc
    crv = objref.Curve()
    if not crv: return Rhino.Commands.Result.Failure
    crv_length = crv.GetLength()
    length = 0
    rc, length = Rhino.Input.RhinoGet.GetNumber("Length from start", True, length, 0, crv_length)
    if rc!=Rhino.Commands.Result.Success: return rc
    pt = crv.PointAtLength(length)
    if pt.IsValid:
        scriptcontext.doc.Objects.AddPoint(pt)
        scriptcontext.doc.Views.Redraw()
    return Rhino.Commands.Result.Success

if __name__=="__main__":
    ArcLengthPoint()
```
{: #py .tab-pane .fade .in}
