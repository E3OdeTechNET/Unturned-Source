# Unturned-Source ESP Box For 3.0

----------------------------------------------------------
Use Visual Studio C#
Hack.cs : 
-----------------------------------------------------------


public class Clairvoyance
{
	private static bool bSearchLock = false;
	private static List<GameObject> lGobj = new List<GameObject>();
	private static List<Vector3> lGobjPos = new List<Vector3>();

	private static int lCount = 0;
	public static IEnumerator SearchW()
	{
		while(true)
		{
			Search();
			yield return new WaitForSeconds(2f);
		}
	}
	public static IEnumerator RefreshW()
	{
		while(true)
		{
			Refresh();
			yield return new WaitForSeconds(1f);
		}
	}
	private static void Search()
	{
		bSearchLock = true;
		Collider[] cA = Physics.OverlapSphere(Camera.main.transform.position, 100.0f);
		lCount = 0;
		
		for (int i = 0; i < cA.Length; i++)
		{
			Collider c = cA[i];
			GameObject go = c.gameObject;
			if ( go.GetComponent<Transform>() )
			{
				try
				{
					lGobj[lCount] = c.gameObject;
				}
				catch
				{
					lGobj.Add(c.gameObject);
				}
				lCount++;
			}
		}
		while (lGobj.Count > lCount) 
       		lGobj.RemoveAt(lGobj.Count-1);
		bSearchLock = false;

	}
	public static void Refresh()
	{
		if (bSearchLock)
			return;
		for (int i = 0; i < lCount; i++)
		{
			GameObject go = lGobj[i];
			Vector3 v = go.transform.position;
			try
			{
				lGobjPos[i] = v;
			}
			catch
			{
				lGobjPos.Add(v);
			}
		}
		while (lGobjPos.Count > lCount) 
       		lGobjPos.RemoveAt(lGobjPos.Count-1);
	}
	public static void Display()
	{
		if (bSearchLock)
			return;
		SleekRender.label(new Rect(0,80,550,20),"Clairvoyance.Display() "+lGobj.Count.ToString()+" "+lGobjPos.Count.ToString()+" / "+lCount.ToString(),String.Empty,Color.white);
		Vector3 ppp = Camera.main.transform.position;
		SleekRender.label(new Rect(0,90,250,20),ppp.x.ToString("n1")+","+ppp.y.ToString("n1")+","+ppp.z.ToString("n1"),String.Empty,Color.white);	
		for (int i = 0; i < lCount; i++)
		{

			Vector3 w = lGobjPos[i];
			String n = lGobj[i].name;
			
			Vector3 v = Camera.main.WorldToScreenPoint(w);
			if (v.z > 0)
			{
				float f = (w-ppp).magnitude;
				SleekRender.label(new Rect(v.x,Screen.height-v.y,200,20),n+" ["+f.ToString("n1")+"]",String.Empty,Color.red);
				
				Renderer[] rA = lGobj[i].GetComponentsInParent<Renderer>();
				foreach(Renderer r in rA)
					Make3DBox(r.bounds.center,r.bounds.size,new Color(0,255,0,Math.Min(0.5f+f/50.0f,1.0f)));
				
			}
		}
		
	}
	private static void MakeESPLine(Vector3 center, float x1, float y1, float z1, float x2, float y2, float z2, Color color)
	{
		Vector3 pointPos1 = new Vector3(center.x + x1, center.y + y1, center.z + z1);
		Vector3 pointPos2 = new Vector3(center.x + x2, center.y + y2, center.z + z2);
		Vector3 xy1 = Camera.main.WorldToScreenPoint(pointPos1);
		Vector3 xy2 = Camera.main.WorldToScreenPoint(pointPos2);
		if ((xy1.z>0)&&(xy2.z>0))
			Drawing.DrawLine(new Vector2(xy1.x,Screen.height-xy1.y),new Vector2(xy2.x,Screen.height-xy2.y),color,1.0f,false);
	}
	private static void Make3DBox(Vector3 center, Vector3 size, Color color)
	{
		//clockwise
		
		//bottom
		MakeESPLine(center, -size.x/2, -size.y/2, size.z/2, size.x/2, -size.y/2, size.z/2, color);
		MakeESPLine(center, size.x/2, -size.y/2, size.z/2, size.x/2, -size.y/2, -size.z/2, color);
		MakeESPLine(center, size.x/2, -size.y/2, -size.z/2, -size.x/2, -size.y/2, -size.z/2, color);
		MakeESPLine(center, -size.x/2, -size.y/2, -size.z/2, -size.x/2, -size.y/2, size.z/2, color);
	
		//middle
		MakeESPLine(center, -size.x/2, -size.y/2, size.z/2, -size.x/2, size.y/2, size.z/2, color);
		MakeESPLine(center, size.x/2, -size.y/2, size.z/2, size.x/2, size.y/2, size.z/2, color);
		MakeESPLine(center, size.x/2, -size.y/2, -size.z/2, size.x/2, size.y/2, -size.z/2, color);
		MakeESPLine(center, -size.x/2, -size.y/2, -size.z/2, -size.x/2, size.y/2, -size.z/2, color);
	
		//top
		MakeESPLine(center, -size.x/2, size.y/2, size.z/2, size.x/2, size.y/2, size.z/2, color);
		MakeESPLine(center, size.x/2, size.y/2, size.z/2, size.x/2, size.y/2, -size.z/2, color);
		MakeESPLine(center, size.x/2, size.y/2, -size.z/2, -size.x/2, size.y/2, -size.z/2, color);
		MakeESPLine(center, -size.x/2, size.y/2, -size.z/2, -size.x/2, size.y/2, size.z/2, color);
	}
}
