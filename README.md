# M1_row-and-coloumn-transposition
ROW TRANSPOSITION
#include&lt;stdio.h&gt;
#include&lt;string.h&gt;
#include&lt;stdlib.h&gt;
main()
{
int i,j,len,rails,count,code[100][1000];
char str[1000];
printf(&quot;Enter a Secret Message\n&quot;);
gets(str);
len=strlen(str);
printf(&quot;Enter number of rails\n&quot;);
scanf(&quot;%d&quot;,&amp;rails);
for(i=0;i&lt;rails;i++)
{
for(j=0;j&lt;len;j++)
{
code[i][j]=0;
}
}
count=0;
j=0;
while(j&lt;len)
{
if(count%2==0)
{

for(i=0;i&lt;rails;i++)
{
code[i][j]=(int)str[j];
j++;
}

}
else
{
for(i=rails-2;i&gt;0;i--)
{
code[i][j]=(int)str[j];
j++;
}
}
count++;
}
for(i=0;i&lt;rails;i++)
{
for(j=0;j&lt;len;j++)
{
if(code[i][j]!=0)
printf(&quot;%c&quot;,code[i][j]);

}
}
printf(&quot;\n&quot;);
}

COLUMN TRANSPOSITION
#include&lt;bits/stdc++.h&gt;
using namespace std;
// Key for Columnar Transposition
string key;
map&lt;int,int&gt; keyMap;
void setPermutationOrder()
{
// Add the permutation order into map
for(int i=0; i &lt; key.length(); i++)
{
keyMap[key[i]] = i;
}
}
// Encryption
string encryptMessage(string msg)
{
int row,col,j;
string cipher = &quot;&quot;;
col = key.length();
/* calculate Maximum row of the matrix*/
row = msg.length()/col;
if (msg.length() % col)
row += 1;
char matrix[row][col];
for (int i=0,k=0; i &lt; row; i++)
{
for (int j=0; j&lt;col; )
{
if(msg[k] == &#39;\0&#39;)
{
/* Adding the padding character &#39;_&#39; */
matrix[i][j] = &#39;_&#39;;
j++;
}
if( isalpha(msg[k]) || msg[k]==&#39; &#39;)
{
/* Adding only space and alphabet into matrix*/
matrix[i][j] = msg[k];
j++;
}
k++;
}
}
for (map&lt;int,int&gt;::iterator ii = keyMap.begin(); ii!=keyMap.end(); ++ii)
{
j=ii-&gt;second;
// getting cipher text from matrix column wise using permuted key
for (int i=0; i&lt;row; i++)
{
if( isalpha(matrix[i][j]) || matrix[i][j]==&#39; &#39; || matrix[i][j]==&#39;_&#39;)
cipher += matrix[i][j];

}
}
return cipher;
}
// Decryption
string decryptMessage(string cipher)
{
/* calculate row and column for cipher Matrix */
int col = key.length();
int row = cipher.length()/col;
char cipherMat[row][col];
/* add character into matrix column wise */
for (int j=0,k=0; j&lt;col; j++)
for (int i=0; i&lt;row; i++)
cipherMat[i][j] = cipher[k++];
/* update the order of key for decryption */
int index = 0;
for( map&lt;int,int&gt;::iterator ii=keyMap.begin(); ii!=keyMap.end(); ++ii)
ii-&gt;second = index++;
/* Arrange the matrix column wise according
to permutation order by adding into new matrix */
char decCipher[row][col];
map&lt;int,int&gt;::iterator ii=keyMap.begin();
int k = 0;
for (int l=0,j; key[l]!=&#39;\0&#39;; k++)
{
j = keyMap[key[l++]];
for (int i=0; i&lt;row; i++)
{
decCipher[i][k]=cipherMat[i][j];
}
}
string msg = &quot;&quot;;
for (int i=0; i&lt;row; i++)
{
for(int j=0; j&lt;col; j++)
{
if(decCipher[i][j] != &#39;_&#39;)
msg += decCipher[i][j];

}
}
return msg;
}
int main(void)
{
string msg;
cin&gt;&gt;msg;
cin&gt;&gt;key;
setPermutationOrder();
string cipher = encryptMessage(msg);
cout &lt;&lt; &quot;Encrypted Message: &quot; &lt;&lt; cipher &lt;&lt; endl;
cout &lt;&lt; &quot;Decrypted Message: &quot; &lt;&lt; decryptMessage(cipher) &lt;&lt; endl;
return 0;
}
