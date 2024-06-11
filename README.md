#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>

void count_file(const char *filename, int *lines, int *words, int *characters) 
{
    FILE *file = fopen(filename, "r");
    if (!file) {
        perror("Could not open file");
        exit(EXIT_FAILURE);
    }

    int c;
    int in_word = 0;
    *lines = *words = *characters = 0;

    while ((c = fgetc(file)) != EOF) 
    {
        (*characters)++;

        if (c == '\n') 
        {
            (*lines)++;
        }

        if (isspace(c)) 
        {
            in_word = 0;
        } else if (!in_word) 
        {
            in_word = 1;
            (*words)++;
        }
    }

    fclose(file);
}

int main(int argc, char *argv[]) 
{
    if (argc != 2) {
        fprintf(stderr, "Usage: %s <filename>\n", argv[0]);
        return EXIT_FAILURE;
    }

    int lines, words, characters;
    count_file(argv[1], &lines, &words, &characters);

    printf("%d %d %d %s\n", lines, words, characters, argv[1]);

    return EXIT_SUCCESS;
}
