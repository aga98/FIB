# Vértices de Corte

---

- [Visited] : for making sure, if we have visited a node(a vertex) or not and not running into an infinite loop .You can understand what I'm saying if you did the DFS.
- [isArticulation] : it is a boolean array to mark if a vertex is Cut-vertex (or Articulation Point)
- [parent] : It keeps the record of parent of each vertex
- [disc] : It answers a simple question, when was a particular vertex " discovered" in the depth- first-search ?, which means it assigns a number to the the vertex in the order it is found in the dfs.
- [low] : It answers yet another simple question, "what is lowest level vertex ,x can climb to, in case its parent is removed from the graph"

![](Untitled-95d69042-8a99-4e25-9569-5be5bf4645d4.png)

A la izquierda de cada nodo aparece la numeración en el orden del recorrido, **disc**, y a la derecha en rojo el número del vértice más alto al que se puede
remontar, **low**. Las aristas continuas son aristas del TDFS y las discontinuas son las aristas back del grafo. Los vértices con numeración en el orden de recorrido
1 y 6 son puntos de articulación.

    // Mark all the vertices as not visited 
    vector<bool> visited; 
    // Stores the disc a visited vertex has been discovered
    vector<int> disc; 
    vector<int> low; 
    vector<int> parent; 
    vector<bool> isArticulation; // To store articulation points 
    int times;
    
    
    void findCut(int u){
        // Count of children in DFS Tree 
        int children = 0;   
        // Mark the current node as visited 
        visited[u] = true;  
        // Initialize discovery time and low value 
        disc[u] = low[u] = ++times; 
    
        for(int i=0; i<G[u].size(); ++i){
            int v = G[u][i];
            // If v is not visited yet, then make it a child of u in DFS tree and recur for it 
            if (not visited[v]) { 
                children++; 
                parent[v] = u; 
                findCut(v); 
                // Check if the subtree rooted with v has a connection to 
                // one of the ancestors of u 
                low[u]  = min(low[u], low[v]); 
                // u is an articulation point in following cases 
                // (1) u is root of DFS tree and has two or more chilren. 
                if (parent[u] == -1 and children > 1) isArticulation[u] = true; 
                // (2) If u is not root and low value of one of its child is more 
                // than discovery value of u. 
                if (parent[u] != -1 and low[v] >= disc[u]) isArticulation[u] = true; 
            } 
            // Update low value of u for parent function calls. 
            else if (v != parent[u]) low[u] = min(low[u], disc[v]); 
        }
    }
    
    
    void findCutVertices(){ 
        // Initialize parent and visited, and ap(articulation point) arrays 
        for (int i = 0; i < V; i++){ 
            parent[i] = -1; 
            visited[i] = false; 
            isArticulation[i] = false; 
        } 
        times = 0:  
        // Call the recursive helper function to find articulation points 
        // in DFS tree rooted with vertex 'i' 
        for (int i = 0; i < V; i++) if(not visited[i]) findCut(i); 
        // Print articulation points
        for (int i = 0; i < V; i++) if(isArticulation[i]) cout << i << " "; 
    }