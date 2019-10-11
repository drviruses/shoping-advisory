# shoping-advisory
# using c++ data file handling 




						
						
						#include<iostream>
						#include<fstream>
						#include<string.h>
						#include<cstdio>                              //for rename() and remove()
						#include<conio.h>
						
						using namespace std;
					/*	char pass[5],passc[5]="@Mit";*/
						
					/*	void confor()                                 //checking for verified user's password
						{
						  cout<<"Hello! We need confirmation! Password Please:- ";
							cin>>pass;
							
						  int n=strcmp(pass,passc);                  //user confirmation
							
							if(n==0)
							{
								cout<<"\n\t\t\t\t\t\tWelcome Amit!!"<<endl;
								return;
							}
							   
							else
							{
							    cout<<"Sorry! You are not granted the access!!"; 
							    exit(1);
						       }
						
						}*/
						
				/* confor() for the authentication for the console screen*/		
						
						int confor()
						
						{
							char pass[5];
							int i;
							string passwd;
							cout<<"Enter the password"<<endl;
							for(i=0;i<4;i++)
							{
							pass[i]=getch();                   /* taking the inputs and displaying '*' as a password does*/
							cout<<"*";
							}
							pass[i]='\0';                      /*to make the input string*/
							/*system("cls");*/                 /*used for clearing the screen, but not using*/
							passwd=pass;
							if(passwd=="@Mit")
								return 1;                      /* returning 1 to the if statement call*/
							else
								return 0;
						}
						
				
						
				/*the processor(metaphor) of the data the data are stores in the blocks of the objects*/		
						
						class shop{
							char pname[26];
							char pcode[10];
							float pcost;
							int pq;
					    public:
						     void input();
							 void display();		 		
                             int checkpc(char ac[])                //accessor function
							 { return strcmp(pcode,ac);
								 }
							 void modify();
						 }s,sh;
						
						 
						 
						 /*input() member function defined outside the class*/
						 void shop::input()
						 {
							 	cout<<"Enter the name of product: "<<endl;
							 	  cin>>pname;
							 	cout<<"Enter the product code: "<<endl;
								   cin>>pcode;
								cout<<"Enter the product cost: "<<endl;
								  cin>>pcost;     
								cout<<"Enter the product quantity: "<<endl;
								  cin>>pq;	 
							}
									 
						 
						 /*display() member function defined outside the class*/
						 void shop::display()
						{
                             	cout<<"\n\t\tName of the product:-"<<pname<<endl;
                             	cout<<"\t\tProduct code of product:-"<<pcode<<endl;
                             	cout<<"\t\tCost of the product:-"<<pcost<<endl;
                             	cout<<"\t\tProduct Quantity in stock:-"<<pq<<endl;
							}
						
						 /* modify() member function defined outside the class*/
						 void shop::modify()
						 {
						 	  	cout<<"\t\t\tProduct Name: "<<pname<<endl;
							  	cout<<"\t\t\tProduct Code: "<<pcode<<endl;
							  	cout<<"\t\t\tProduct Cost: "<<pcost<<endl;
							  	cout<<"\t\t\tProduct Quantity: "<<pq<<endl;
							  	
							  	char pn[10]=" ",pc[10]=" ";
							  	float pcs; int pqq;
							  	
							  	cout<<"\t\tNew Product Name:(Enter '.' to retain old one)";
							  	  cin>>pn;
							  	cout<<"\t\tNew Product code:(Enter '.' to retain old one)";
								  cin>>pc;
								cout<<"\t\tNew Product cost:(Enter '0' to retain old one)";
								  cin>>pcs;
								cout<<"\t\tNew Product quantity:(Enter '0' to retain old one)";
								  cin>>pqq;
								
								if(strcmp(pn,".")!=0)
								   strcpy(pname,pn);
								if(strcmp(pc,".")!=0)
								   strcpy(pcode,pc);          
							  	if(pcs!=0)
								   pcost=pcs;
								if(pqq!=0)
								   pq=pqq;
						 	
						    }
						
						
						/*function to add a items details to the file*/
						
						 void add()
						 {
						 	ofstream fo;
						 	fo.open("items.dat",ios::app|ios::binary);
						 	char ans='y';
						 	while((ans=='y')||(ans=='Y'))
							{
								s.input();
								fo.write((char*)&s,sizeof(s));
								cout<<"Item added to file!!\n";
								cout<<"Want to Add more items(y/n):- ";
								  cin>>ans;
							}
							
							fo.close();
						 	
						   }
						 
						 
						/*funtion for modification of the product from the file*/ 
						 
						 void firmod()
						 {
						 	
                            fstream fio;
							fio.open("items.dat",ios::in|ios::out|ios::binary);
							char mpc[10];
							long pos; char found='f';
							cout<<"\nEnter the Product code to be modified:-";
							  cin>>mpc;
							while(!fio.eof())
							{
								pos=fio.tellg();  //before reading a record its begining pos is determined with tellg()
								fio.read((char*)&s,sizeof(s));
								if(s.checkpc(mpc)==0)
								{
									s.modify();
									fio.seekg(pos);
									fio.write((char*)&s,sizeof(s));
									found='t';
									break;
								}
							  }  
							  
							  if(found=='f')
							    cout<<"\nRecord not found!!\n";
							  fio.close();						 	
						   }
						   
						   
						  /*function to delete the product from the file*/
						   
						  void delt()
						  {
						    ifstream fio("items.dat",ios::binary);
						    ofstream file("temp.dat",ios::out|ios::binary);
						    char mpc1[10];
						    char found='f',confirm='n';
						    cout<<"\nEnter the Product code to be Deleted:-";
							  cin>>mpc1;
							while(!fio.eof())
							{
								fio.read((char*)&s,sizeof(s));
								if(s.checkpc(mpc1)==0)
								{
									s.display();
									found='t';
									cout<<"Are you sure , you want to delete this record(y/n):.. ";
									  cin>>confirm;
									if(confirm=='n')
									  file.write((char*)&s,sizeof(s));  
								}
								else
								     file.write((char*)&s,sizeof(s));
							    
							   } 
								if(found=='f')
								 cout<<"\nRecord not found!!\n";
								fio.close();
								file.close();
								remove("items.dat");
								rename("temp.dat","items.dat");   //old file is removed and temp file is renamed
								 		  
							  }
							
					
					   /*funtion to display the whole data from the file*/
					
						void dis()
						{
							cout<<"\nThe file contains:\n";
							ifstream fio("items.dat");
							while(!fio.eof())
							
							{
							  fio.read((char*)&sh,sizeof(sh));
							  if(fio.eof())
							    break;
							  sh.display();		
							  }
							fio.close();
							  
						 }
						 
						
						
						/*function for displaying the particular item from the data*/ 
						 
						 void pshow()
						 {
						 	ifstream fio("items.dat",ios::binary);
						    
						    char pc1[10];
						    char found='f',confirm='n';
						    cout<<"\nEnter the Product code to be Shown:-";
							  cin>>pc1;
							while(!fio.eof())
							{
								fio.read((char*)&s,sizeof(s));
								if(s.checkpc(pc1)==0)
								{
									s.display();
									found='t';
									  
								}
							    
							   } 
								if(found=='f')
								 cout<<"\nRecord not found!!\n";
								fio.close();
								
						 	}
							 
							 	
		
			/* the heart of the program the main()*/
							 
						int main()
						{
							
							cout<<"****************************Welcome to Amit's Shop**************************\n";
							cout<<"*****************All Products available here***********************************\n";
							
							
							if(confor())
						{
								int c;
							
							while(c!=6)
								{
								
								cout<<"\nEnter your choice: \n";	
								
								
								cout<<"\t\t\t1. Add Items"<<endl;
								cout<<"\t\t\t2. Modify the Data"<<endl;
								cout<<"\t\t\t3. Delete an item"<<endl;
								cout<<"\t\t\t4. Show Data of all items:"<<endl;
								cout<<"\t\t\t5. Show Data of particular item:"<<endl;
								cout<<"\t\t\t6. Exit"<<endl;
								cin>>c;
							    
							    
								switch(c)
							    {
							    	case 1:  add();
							    	         break;
							    	case 2:  firmod();
									         break;
									case 3:  delt();
									         break;
									case 4:  dis();
									         break;
									case 5:  pshow();
									         break;
									case 6:  exit(1);
									         break;          
									default: cout<<"\n\t\tSorry! Wrong input!!";
								}
							}
					    }
					    
						else
					     cout<<"\n\t\tYou are not granted the access!! Wrong Password!!";
							
							
						   return 0;
						}

