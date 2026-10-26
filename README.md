# Activity-Challenge-17
#include <stdio.h>
#include <string.h>

#define MAX 20

int graph[MAX][MAX], n;
char names[MAX][50];

int findNode(char target[]) {
    for (int i = 0; i < n; i++)
        if (strcmp(names[i], target) == 0)
            return i;
    return -1;
}

int bfs(int start, int target) {
    int visited[MAX] = {0}, queue[MAX];
    int front = 0, rear = 0, found = 0;
    visited[start] = 1;
    queue[rear++] = start;
    printf("BFS traversal: ");
    while (front < rear) {
        int u = queue[front++];
        printf("%s ", names[u]);
        if (u == target) found = 1;
        for (int v = 0; v < n; v++) {
            if (graph[u][v] && !visited[v]) {
                visited[v] = 1;
                queue[rear++] = v;
            }
        }
    }
    printf("\nBFS result: %s\n", found ? "File found" : "File not found");
    return found;
}

void dfsVisit(int u, int visited[], int target, int *found) {
    visited[u] = 1;
    printf("%s ", names[u]);
    if (u == target) *found = 1;
    for (int v = 0; v < n; v++)
        if (graph[u][v] && !visited[v])
            dfsVisit(v, visited, target, found);
}

int dfs(int start, int target) {
    int visited[MAX] = {0}, found = 0;
    printf("DFS traversal: ");
    dfsVisit(start, visited, target, &found);
    printf("\nDFS result: %s\n", found ? "File found" : "File not found");
    return found;
}

int main(void) {
    int edges, u, v;
    char startName[50], targetName[50];

    printf("Enter number of nodes: ");
    scanf("%d", &n);
    if (n < 1 || n > MAX) {
        printf("Invalid number of nodes.\n");
        return 1;
    }

    printf("Enter file/directory names (single words):\n");
    for (int i = 0; i < n; i++)
        scanf("%49s", names[i]);

    printf("Enter number of connections: ");
    scanf("%d", &edges);
    for (int i = 0; i < edges; i++) {
        char a[50], b[50];
        printf("Enter connected node names: ");
        scanf("%49s %49s", a, b);
        u = findNode(a);
        v = findNode(b);
        if (u == -1 || v == -1) {
            printf("Unknown node name. Try again.\n");
            i--;
        } else {
            graph[u][v] = graph[v][u] = 1;
        }
    }

    printf("Enter starting node: ");
    scanf("%49s", startName);
    printf("Enter file to search: ");
    scanf("%49s", targetName);
    u = findNode(startName);
    v = findNode(targetName);
    if (u == -1 || v == -1) {
        printf("Starting node or target file does not exist in the graph.\n");
        return 1;
    }
    bfs(u, v);
    dfs(u, v);
    return 0;
}
