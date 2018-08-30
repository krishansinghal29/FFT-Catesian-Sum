# FFT-Catesian-Sum
/*
// Sample code to perform I/O:
#include <stdio.h>

int main(){
	int num;
	scanf("%d", &num);              			// Reading input from STDIN
	printf("Input number is %d.\n", num);       // Writing output to STDOUT
}

// Warning: Printing unwanted or ill-formatted data to output will cause the test cases to fail
*/

// Write your code here
#include <iostream>
#include <vector>
#include <math.h>
using namespace std;

vector<vector<float>> rev_fft(vector <vector<float>> A,int d,int n){
    vector <float> real(n);
    vector <float> img(n);
    vector<vector<float>> C(2);
    C[0]=vector<float>(n);
    C[1]=vector<float>(n);
    if(n==1){
        C[0][0]=0.0;C[1][0]=0.0;
        for(int i=0;i<d;i++){
            C[0][0]=C[0][0]+A[0][i];
            C[1][0]=C[1][0]+A[1][i];
        } 
        return C;
    }
    else{  
        vector<vector<float>> A1(2);
        vector<vector<float>> A2(2);
        A1[0]=vector<float>(d-d/2);
        A2[0]=vector<float>(d/2);
        A1[1]=vector<float>(d-d/2);
        A2[1]=vector<float>(d/2);
        int i=0;
        for(i=0;i<d/2;i++){
            A1[0][i]=A[0][2*i];
            A2[0][i]=A[0][2*i+1];
            A1[1][i]=A[1][2*i];
            A2[1][i]=A[1][2*i+1];
        }
        if(i<d-d/2){
            A1[0][i]=A[0][d-1];
            A1[1][i]=A[1][d-1];
        }
        vector<vector<float>> a1(2);
        a1[0]=vector<float>(n/2);
        a1[1]=vector<float>(n/2);
        a1=rev_fft(A1,d-d/2,n/2);
        vector<vector<float>> a2(2);
        a2[0]=vector<float>(n/2);
        a2[1]=vector<float>(n/2);
        a2=rev_fft(A2,d/2,n/2);
        vector <float> real(n);
        vector <float> img(n);
        vector<float> w(2);
        for(int i=0;i<n/2;i++){
            w[0]=cos(2*M_PI*i/n);
            w[1]=-sin(2*M_PI*i/n);
            real[i]=a1[0][i]+w[0]*a2[0][i]-w[1]*a2[1][i];
            img[i]=a1[1][i]+w[0]*a2[1][i]+w[1]*a2[0][i];
            real[i+n/2]=a1[0][i]-(w[0]*a2[0][i]-w[1]*a2[1][i]);
            img[i+n/2]=a1[1][i]-(w[0]*a2[1][i]+w[1]*a2[0][i]);
        }
    
        C[0]=real;
        C[1]=img;
        return C;
    }
}

vector<vector<float>> fft(vector <int> A,int d,int n){
    vector <float> real(n);
    vector <float> img(n);
    vector<vector<float>> C(2);
    C[0]=vector<float>(n);
    C[1]=vector<float>(n);
    if(n==1){
        C[0][0]=0;
        C[1][0]=0;
        for(int i=0;i<d;i++){
            C[0][0]=1.00*C[0][0]+1.00*A[i];
            C[1][0]=0.0;
        } 
        return C;
    }
    else{    
        vector<int> A1(d-d/2);
        vector<int> A2(d/2);
        int i=0;
        //printf("%d ",A[d]);
        for(i=0;i<d/2;i++){
            A1[i]=A[2*i];
            A2[i]=A[2*i+1];
        }
        if(i<d-d/2){
            A1[i]=A[d-1];
            //printf("%d ",A1[i]);
        }
        vector<vector<float>> a1(2);
        a1[0]=vector<float>(n/2);
        a1[1]=vector<float>(n/2);
        a1=fft(A1,d-d/2,n/2);
        vector<vector<float>> a2(2);
        a2[0]=vector<float>(n/2);
        a2[1]=vector<float>(n/2);
        a2=fft(A2,d/2,n/2);
        
        vector <float> real(n);
        vector <float> img(n);
        vector<float> w(2);
        for(int i=0;i<n/2;i++){
            w[0]=cos(2*M_PI*i/n);
            w[1]=sin(2*M_PI*i/n);
            real[i]=a1[0][i]+w[0]*a2[0][i]-w[1]*a2[1][i];
            img[i]=a1[1][i]+w[0]*a2[1][i]+w[1]*a2[0][i];
            real[i+n/2]=a1[0][i]-(w[0]*a2[0][i]-w[1]*a2[1][i]);
            img[i+n/2]=a1[1][i]-(w[0]*a2[1][i]+w[1]*a2[0][i]);
        }
        C[0]=real;
        C[1]=img;
        return C;
    }
}

int main(){
	int numtest;
	cin >> numtest;								
	for(int i=0 ;i<numtest;i++){
	    int num;
	    cin >> num;
	    vector<int> A(10*num+1);
	    //int old=0;
	    for(int i=0;i<10*num+1;i++){
	        A[i]=0;
	    }
        for(int i=0;i<num;i++){
            int in;
	        cin >> in;
	        /*if(in>old){
	            old=in;
	        }*/
	        A[in]=A[in]+1;
	    }
        //for(int i=0;i<10*num+1;i++){
           // printf("%d ",A[i]);
        //}
	    vector<int> B(10*num+1);
	    for(int i=0;i<10*num+1;i++){
	        B[i]=0;
	    }
        for(int i=0;i<num;i++){
            int in;
	        cin >> in;
	        //if(in>old1){
	          //  old1=in;
	        //}
	        B[in]=B[in]+1;
	    }

	    int nth=pow(2,floor(log2(20*num))+1);
	    //printf("%d",nth);
	    vector<vector<float>> a1(2);
        a1[0]=vector<float>(nth);
        a1[1]=vector<float>(nth);
        a1=fft(A,10*num+1,nth);
        //printf("%f ",a1[0][35]);
        vector<vector<float>> a2(2);
        a2[0]=vector<float>(nth);
        a2[1]=vector<float>(nth);
        a2=fft(B,10*num+1,nth);
        
        vector <float> real(nth);
        vector <float> img(nth);
        for(int i=0;i<nth;i++){
            real[i]=a1[0][i]*a2[0][i]-a1[1][i]*a2[1][i];
            img[i]=a1[0][i]*a2[1][i]+a1[1][i]*a2[0][i];
        }
        //("%f ",img[6]);
        vector<vector<float>> C(2);
        C[0]=vector<float>(nth);
        C[1]=vector<float>(nth);
        vector<vector<float>> C1(2);
        C1[0]=vector<float>(nth);
        C1[1]=vector<float>(nth);
        C[0]=real;
        C[1]=img;
        C1=rev_fft(C,nth,nth);
        //printf("%f ",C1[0][20]);
        int count=0;
        for(int i=0;i<nth;i++){
            int p;
            p=round(C1[0][i]/nth);
            //float p;
            //p=C1[0][i];
            if(p!=0){
                count++;
                //printf("%d %f\n",i,p);
            }
        }
        printf("%d\n",count);
        for(int i=0;i<nth;i++){
            int p;
            p=round(C1[0][i]/nth);
            //float p;
            //p=C1[0][i];
            if(p!=0){
                printf("%d %d\n",i,p);
                //printf("%d %f\n",i,p);
            }
        }
	}	
	return 0;
}
