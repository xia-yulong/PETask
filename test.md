## h2

#### h4

https://github.com/xia-yulong/PETask/blob/main/README.md

https://www.jlu.edu.cn/

https://img2.baidu.com/it/u=3024878745,545382191&fm=26&fmt=auto&gp=0.jpg

code block：

#define DEBUG

#include <stdlib.h>

#ifdef DEBUG
#include <stdio.h>
#endif

#define __malloc(x) malloc(x)
#define __free(x) free(x)
#define __assert(x)


#ifdef DEBUG
static void __kmp_test(unsigned char *W, unsigned int wlen, unsigned int *T)
{
    unsigned int i=0;
    printf("i:\tW[i]\tT[i]\n");
    while (i < wlen) {
        printf("%d:\t%c\t%d\n", i, W[i], T[i]);
        i++;
    }
}
#endif

static void __kmp_table(unsigned char *W, unsigned int wlen, unsigned int *T)
{
    unsigned int pos=2, cnd=0;
    T[0]=-1;
    T[1]=0;
    while (pos < wlen) {
        if (W[pos-1] == W[cnd]){
            cnd = cnd+1;
            T[pos] = cnd;
            pos = pos+1;
        }
        else if (cnd > 0) {
            cnd = T[cnd];
        }
        else {
            T[pos]=0;
            pos=pos+1;
        }
    }
}

unsigned int kmp_search(unsigned char *S, unsigned int slen,
                        unsigned char *W, unsigned int wlen)
{
    unsigned int m=0, i=0;
    unsigned int *T;
    __assert(S && W);
    
    T = (unsigned int*)__malloc(wlen * sizeof(unsigned int));
    __assert(T);
    __kmp_table(W, wlen, T);

#ifdef DEBUG
    __kmp_test(W, wlen, T);
#endif

    while (m+i < slen) {
        if (W[i] == S[m+i]) {
            if (i == wlen-1)
            {
                __free(T);
                return m;
            }
            i = i+1;
        }
        else {
            m = m+i-T[i];
            if (T[i] > -1)
                i = T[i];
            else
                i = 0;
        }
    }
    __free(T);
    return slen;
}