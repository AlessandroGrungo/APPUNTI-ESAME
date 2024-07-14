# APPUNTI-ESAME
CREAZIONE GRAFO

CONTROLLER

self._model.buildGraph(s, a)
        self._view.txt_result.controls.append(ft.Text(f"Numero di vertici: {self._model.get_num_of_nodes()} Numero di archi: {self._model.get_num_of_edges()}"))


        for p in self._model.get_sum_weight_per_node():
             self._view.txt_result.controls.append(ft.Text(f"Nodo {p[0]}, somma pesi su archi ={p[1]}"))

        self._view.update_page()



MODEL
IMPORT:
import networkx as nx

INIZIO:
self._grafo = nx.Graph()
self._nodes = []
self._edges = []

FUNZIONE CHE CREA IL GRAFO
 def buildGraph(self, c, a):
        self._grafo.clear()
        #CONDIZIONI
        self._nodes.append(NODO)

        self._grafo.add_nodes_from(self._nodes)
        self.idMap = {}
        for n in self._nodes:
            self.idMap[n.(ID)] = n

#CONDIZIONI TRA NODI SU SQL MA CREAZIONE ARCHI IN MODEL
        self._grafo.add_edge(n1, n2, weight=???)
#ALTRIMENTI ESTRAI IN SQL LE CONDIZIONI E GLI ARCHI E POI USI:
        self._grafo.add_weighted_edges_from(self.edges)


ALTRE FUNZIONI UTILI
    def get_nodes(self):
        return self._grafo.nodes()

    def get_edges(self):
        return list(self._grafo.edges(data=True))

    def get_num_of_nodes(self):
        return self._grafo.number_of_nodes()

    def get_num_of_edges(self):
        return self._grafo.number_of_edges()

    def get_sorted_edges(self):
        return sorted(self._grafo.edges(data=True), key=lambda x: x[2]['weight'], reverse=True)

    def getNumConfinanti(self, v):
        return len(list(self._graph.neighbors(v)))

    def getNumCompConnesse(self):
        # S = [self._graph.subgraph(c).copy() for c in nx.connected_components(self._graph)] #this is to get all the components
        return nx.number_connected_components(self._graph)
