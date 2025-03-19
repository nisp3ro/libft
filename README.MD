# 📚 Libft 125/100
First project of the 42's Cursus.

The aim of this project is to code a C library regrouping usual functions that you'll be allowed to use in all your other projects.

## Installation & Compilation

To compile Libft and generate the static library `libft.a`, run:

```sh
make
```

This will produce `libft.a`, which can be linked to your projects.

### Cleaning Up

- `make clean` – Removes object files.
- `make fclean` – Removes object files and `libft.a`.
- `make re` – Fully recompiles the library.

## Usage

To use Libft in your project, include the header file and link the library:

```c
#include "libft.h"
```

Compile your program with:

```sh
cc my_program.c -L. -lft -o my_program
```

## Functions

### **Mandatory Part (Libc Functions)**

- **Character Handling:**

  - `ft_isalpha(int c)`: Checks if a character is alphabetic.
  - `ft_isdigit(int c)`: Checks if a character is a digit.
  - `ft_isalnum(int c)`: Checks if a character is alphanumeric.
  - `ft_isascii(int c)`: Checks if a character is an ASCII character.
  - `ft_isprint(int c)`: Checks if a character is printable.
  - `ft_toupper(int c)`: Converts a character to uppercase.
  - `ft_tolower(int c)`: Converts a character to lowercase.

- **String Manipulation:**

  - `ft_strlen(const char *str)`: Returns the length of a string.
  - `ft_strchr(const char *s, int c)`: Finds the first occurrence of a character.
  - `ft_strrchr(const char *s, int c)`: Finds the last occurrence of a character.
  - `ft_strncmp(const char *s1, const char *s2, size_t n)`: Compares two strings up to `n` characters.
  - `ft_strlcpy(char *dst, const char *src, size_t dsize)`: Copies a string with size protection.
  - `ft_strlcat(char *dst, const char *src, size_t dsize)`: Concatenates a string with size protection.
  - `ft_strnstr(const char *haystack, const char *needle, size_t len)`: Locates a substring in a string.
  - `ft_strdup(const char *s1)`: Duplicates a string.

- **Memory Management:**

  - `ft_memset(void *b, int c, size_t len)`: Fills memory with a byte.
  - `ft_bzero(void *s, size_t n)`: Zeroes out memory.
  - `ft_memcpy(void *dst, const void *src, size_t n)`: Copies memory.
  - `ft_memmove(void *dst, const void *src, size_t len)`: Moves memory.
  - `ft_memchr(const void *s, int c, size_t n)`: Finds a byte in memory.
  - `ft_memcmp(const void *s1, const void *s2, size_t n)`: Compares two memory blocks.
  - `ft_calloc(size_t count, size_t size)`: Allocates and zeroes memory.

- **Conversions:**

  - `ft_atoi(const char *str)`: Converts a string to an integer.

### **Mandatory Part (Additional Functions)**

- **String Operations:**

  - `ft_substr(char const *s, unsigned int start, size_t len)`: Extracts a substring.
  - `ft_strjoin(char const *s1, char const *s2)`: Concatenates two strings.
  - `ft_strtrim(char const *s1, char const *set)`: Trims characters from a string.
  - `ft_split(char const *s, char c)`: Splits a string into an array of words.
  - `ft_itoa(int n)`: Converts an integer to a string.
  - `ft_strmapi(char const *s, char (*f)(unsigned int, char))`: Maps a function to a string.
  - `ft_striteri(char *s, void (*f)(unsigned int, char *))`: Applies a function to each character.

- **File Descriptors:**

  - `ft_putchar_fd(char c, int fd)`: Writes a character to a file descriptor.
  - `ft_putstr_fd(char *s, int fd)`: Writes a string to a file descriptor.
  - `ft_putendl_fd(char *s, int fd)`: Writes a string with a newline.
  - `ft_putnbr_fd(int n, int fd)`: Writes an integer to a file descriptor.

### **Bonus Part (Linked List Functions)**

- `ft_lstnew(void *content)`: Creates a new list node.
- `ft_lstadd_front(t_list **lst, t_list *new)`: Adds a node at the front.
- `ft_lstsize(t_list *lst)`: Counts the number of nodes.
- `ft_lstlast(t_list *lst)`: Returns the last node.
- `ft_lstadd_back(t_list **lst, t_list *new)`: Adds a node at the end.
- `ft_lstdelone(t_list *lst, void (*del)(void *))`: Deletes a node.
- `ft_lstclear(t_list **lst, void (*del)(void *))`: Deletes all nodes.
- `ft_lstiter(t_list *lst, void (*f)(void *))`: Iterates a function over nodes.
- `ft_lstmap(t_list *lst, void *(*f)(void *), void (*del)(void *))`: Maps a function to a list.

### **Extras**

- `ft_isspace(int c)`: Checks if a character is whitespace.
- `ft_trackalloc(size_t size)`: Allocates memory and tracks it to prevent leaks.
- `ft_trackedfree(void *ptr)`: Frees memory while ensuring it was allocated through `ft_trackalloc`.
- `ft_clearallocs(void)`: Frees all tracked allocations, useful for cleaning up before program termination.

### **Notes**

The `ft_isspace(int c)` function checks if a character is a whitespace character (`' '`, `\t`, `\n`, `\v`, `\f`, `\r`).

The memory tracking functions (`ft_trackalloc`, `ft_trackedfree`, and `ft_clearallocs`) help manage dynamic memory and prevent leaks:
- `ft_trackalloc` works like `malloc` but registers all allocations. 
- `ft_trackedfree` ensures that only tracked memory is freed, preventing double free errors. 
- `ft_clearallocs` is useful at the end of a program to release all remaining allocations, avoiding memory leaks.

Example usage:
```c
// Allocate some memory blocks
void *ptr1 = ft_trackalloc(32);
void *ptr2 = ft_trackalloc(64);
void *ptr3 = ft_trackalloc(128);

// Check if allocation was successful
// If any allocation fails, free all previously allocated blocks.
if (!ptr1 || !ptr2 || !ptr3)
{
    ft_clearallocs();
    return (1);
}

// Free one block correctly
ft_trackedfree(ptr2);

// Attempt to free an already freed pointer 
// (should display a warning)
ft_trackedfree(ptr2);

// Attempt to free a pointer that was never allocated through ft_trackalloc
// (should display a warning)
void *fake_ptr = malloc(50);
ft_trackedfree(fake_ptr);
free(fake_ptr); // Manually free since it's not tracked

// Allocate another block
void *ptr4 = ft_trackalloc(256);
// Overwrite it with another allocation (the previous one will still be tracked)
ptr4 = ft_trackalloc(sizeof(char) * (42 + 1));

// Free all remaining allocated memory, including overwritten pointers
ft_clearallocs();
```



## License

This project is part of 42 School curriculum.
