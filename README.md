# freefem-with-gnuplot

<HTML>

<i><H1>Working with gnuplot</H1></i>
<h2>Problem: PDE</h2>
<p>We consider the following problem:</p>
<ul>
<p>(-Delta + 1)u=f in Omega, Delta=d^2/dx^2+d^2/dy^2</p>
$ (-\Delta+1)u=f \quad {\rm in } \Omega, \Delta=\partial^2/\partial x^2+\partial^2/\partial y^2 $
  <p>u=g on the boundary of Omega</p>
</ul>
<img width="616" height="616" alt="image" src="https://github.com/user-attachments/assets/38974172-ac6c-4cbc-ab34-85a52722d6e0" />

<img width="616" height="616" alt="image" src="https://github.com/user-attachments/assets/b8bb64b6-bb50-42a7-9900-adac336dca2a" />


<h2>Solving PDE and generating gnuplot data by Frefem++</h2>
<p>We choose the following f and g: f=-sin(x*y), g=0.</p>
<h3>Freefem code</h3>
<pre>
real rad=1.0;
int n1;
func f=-10*sin(x*y);
func g=0;
n1=64;

//   define borders
border a(t=0,2*pi){x=rad*(1-0.5*sin(2*t))*cos(t);y=rad*(1-0.25*cos(3*t))*sin(t);label=1;};

//   define domain and mesh
mesh Omega=buildmesh(a(n1));
savemesh(Omega,"./wwg.msh");
//plot(Omega,ps="./Omega.eps");

//generate fem space
fespace Vh(Omega,P1);
Vh u,v;

//define and solve problem
solve pde(u,v)=
  int2d(Omega)(dx(u)*dx(v)+dy(u)*dy(v)+u*v)+on(a,u=g)-int2d(Omega)(f*v);

plot(Omega,u,fill=1,wait=true,ps="u.eps");

//generate gnuplot data
{ofstream ff("u.txt"); 
for(int i=0;i&lt;Omega.nt;i++)
 {for (int j=0; j&lt;3; j++)
ff&gt;&gt;Omega[i][j].x &gt;&gt; " " &gt;&gt; Omega[i][j].y &gt;&gt; " " &gt;&gt; u[][Vh(i,j)] &gt;&gt; endl; 
ff&gt;&gt;Omega[i][0].x &gt;&gt; " " &gt;&gt; Omega[i][0].y &gt;&gt;" " &gt;&gt; u[][Vh(i,0)] &gt;&gt;" \n\n\n";
 } 
} 
</pre>
<img width="616" height="616" alt="image" src="https://github.com/user-attachments/assets/cd271c79-9502-41d5-b624-77d4199106d6" />

<h2>gnuplot operation</h2>
<pre>
gnuplot&gt; set palette rgbformulae 30,31,32
gnuplot&gt; splot "u.txt" w l pal
gnuplot&gt; set term png
Terminal type set to 'png'
Options are 'nocrop medium '
gnuplot&gt; set output "u.png"
gnuplot&gt; replot
smooth palette in png: available 157 color positions; using 157 of them
</pre>
<img width="640" height="480" alt="image" src="https://github.com/user-attachments/assets/d3826b68-8914-4a5e-baf8-ab3738b9fb61" />


</BODY>

</HTML>
