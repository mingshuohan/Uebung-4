# Uebung-4
namespace Uebung


{


    


    public partial class Form1 : Form


    {


        SldWorks.SldWorks swApp;


        public Form1()


        {


            InitializeComponent();


            try


            {


                swApp = (SldWorks.SldWorks)Marshal.GetActiveObject("SolidWorks Application");



            }


            catch(Exception e)


            {


                swApp=new SldWorks.SldWorks();


                swApp.Visible = true;


            }


        }



        private void button1_Click(object sender, EventArgs e)


        {


            ModelDoc2 swModel;


            Sketch swSketch;


            SketchSegment skSeg;


            Feature Feat;


            Feature subFeat;


            object[] Segments;


            SketchPoint cPoint;



            swModel = (ModelDoc2)swApp.ActiveDoc;



            Feat = (Feature)swModel.FirstFeature();



            while(Feat!=null)


            {


                listBox1.Items.Add(Feat.Name + "" + Feat.GetTypeName());



                if(Feat.GetTypeName()=="Cut"&&Feat.Name=="Bohrbild")


                {


                    subFeat = (Feature)Feat.GetFirstSubFeature();



                    while(subFeat!=null)


                    {


                        string FeatureName = subFeat.Name;


                        string FeatureTyp = subFeat.GetTypeName();


                        listBox2.Items.Add(FeatureName + "\t" + FeatureTyp);



                        if(subFeat.GetTypeName()=="ProfileFeature")


                        {


                            swSketch = (Sketch)subFeat.GetSpecificFeature2();


                            Segments = (object[])swSketch.GetSketchSegments();



                            foreach (Object sgObj in Segments)


                            {


                                skSeg = (SketchSegment)sgObj;



                                if (skSeg.GetType() == (int)swSketchSegments_e.swSketchARC)


                                {


                                    SketchArc arc = (SketchArc)skSeg;


                                    double rad = arc.GetRadius();



                                    cPoint = (SketchPoint)arc.GetCenterPoint2();


                                    double x = cPoint.X;


                                    double y = cPoint.Y;



                                    listBox3.Items.Add(x+" "+y);


                                    listBox4.Items.Add(rad);


                                    


                                }



                            }


                            


                        }


                        subFeat = (Feature)subFeat.GetNextFeature();


                    }


                   


                } 


                Feat = (Feature)Feat.GetNextFeature();


            }



　


        }


    }


}
