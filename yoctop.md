#include <stdio.h>

int main() {
    int i;   // NOT volatile

    printf("Without volatile:\n");
    for (i = 0; i < 5; i++) {
        printf("i = %d\n", i);
    }
    return 0;
}
