

# APPUNTI-ESAME

CONTROLLER

        self._view.update_page()

        self._view.valueDD.value
                            
        self._view.dropdd.options.append(ft.dropdown.Option(......))
        
        self._view.txt_result.controls.append(ft.Text(f"........"))
        
        self._view.txt_result.controls.clear()
        

FUNZIONE CHE SVUOTA IL GRAFO

        self._grafo.clear()


FUNZIONE CHE MAPPA I NODI

        self.idMap = {}

        for node in self._nodes:

                self.idMap[ node.ID ] = n

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
                return nx.number_connected_components(self._graph)


        self.maxCompConn = set()
        self.numMaxCompConn = 0
        def maxCompConnessa(self):
                lista = list(nx.connected_components(self.grafo))
                self.numMaxCompConn = 0
                self.maxCompConn = set()
                for compConn in lista:
                    if len(compConn) > self.numMaxCompConn:
                        self.maxCompConn = set(compConn)
                        self.numMaxCompConn = len(compConn)

SE DEVO LEGGERE UN DROPDOWN CHE HA COME DATI UNA CLASSE:

        inserisci nelle iniziali il parametro:        self.choice = None

        nel dropdown inserisci:        (data=oggetto, on_click=self.readDD, text=come vuoi chiamarlo)

        def readDD(self, e):
                if e.control.data == None:
                    self.choice = None
                else:
                    self.choice = e.control.data

        Nell'handle lo richiami con:
                oggetto = self.choice

SE DEVO COMPIERE UN AZIONE QUANDO CAMBIA IL VALORE DI UN DROPDOWN (PER ESEMPIO POPOLARE UN ALTRO DROPDOWN)
Inserisco il comando nella funzione che popola il primo Dropdown:

        def fillDD1(self):
                for a in self._model.list1:
                    self._view.dd1.options.append(ft.dropdown.Option(a))
                self._view.dd1.on_change = self.fillDD2 # comando che chiama funzione che aggiorna altro DD
                self._view.update_page()
                
        def fillDD2(self, e):
                self._view.dd2.options.clear() # pulisci quello che era già dentro al DD
                valore1 = int(self._view.dd2.value) #valore selezionato da dd1 che influenza il dd2
                self._model.load2(valore) # funzione che prende i dati del dd2 associati alla selezione del dd1 (valore1)
                for valore2 in self._model.list2:
                    self._view.ddstate.options.append(ft.dropdown.Option(data=valore2, on_click=self.readDD2, text=s.DatoDaLeggere))
                self._view.update_page() # deve riaggiornare sempre

STRUTTURE UTILI PER FARE VELOCE NELLE CONDIZIONI DA RISPETTARE

        if self._model.get_num_of_nodes() == 0:
                self._view.txt_result.append(ft.Text(f"Il grafo non è stato creato correttamente."))
                return
                
        if self._model.get_num_of_nodes() == 0:
                self._view.create_alert("Il grafo non è stato creato correttamente.")
                return
                
        def verificaValore(self, valore): (poi nella funzione metto if verificaValore = False: return)
                try:
                    float(valore) / int(valore)
                except ValueError:
                    self._view.create_alert("Inserisci un valore numerico.")
                    return False
                if float(valore) > max or float(valore) < min:
                    self._view.create_alert(
                        f"Inserisci numero compreso nell'intervallo (min, max).")
                    return False
                return True

CONTROLLA SE ESISTE UN PERCORSO TRA DUE NODI

        def verificaConnessione(self, a1, a2):
                connessa = nx.node_connected_component(self.graph, a1)
                if a2 in connessa:
                    return True
                return False

RICORSIONE SU ARCHI DOVE CERCO IL PERCORSO DI PESO MAX (CAMMINO SEMPLICE)

        def calcPath(self, soglia):
                self.solBest = []
                self.maxPeso = 0
                for node in self.nodes:
                    self.ricorsione(node, [])
        
        def ricorsione(self, nStart, parzialeArchi):
                archiAmm = self.cercaArchiAmm(nStart, parzialeArchi)
                if not archiAmm:
                    pesoRaggiunto = self.calcPesoPercorso(parzialeArchi)
                    if pesoRaggiunto > self.maxPeso:
                        self.maxPeso = pesoRaggiunto
                        self.solBest = list(parzialeArchi)
                    return
                for arco in archiAmm:
                    nNext = arco[1]
                    parzialeArchi.append((nStart, nNext))
                    self.ricorsione(nNext, parzialeArchi)
                    parzialeArchi.pop()
        
        def cercaArchiAmm(self, nStart, parzialeArchi):
                result = []
                for node in self.grafo.neighbors(nStart):
                    if (nStart,node) not in parzialeArchi and (node, nStart) not in parzialeArchi:
                            result.append((nStart, node))
                return result

        def calcPesoPercorso(self, parzialeArchi):
                peso = 0
                for edge in parzialeArchi:
                    peso += self.grafo[edge[0]][edge[1]]['weight']
                return peso

RICORSIONE SU NODI DOVE CERCO IL PERCORSO DI PESO MAX (CAMMINO SEMPLICE)

        def calcPath(self):
                self.solBest = []
                self.pesoMax = 0
                for node in self.nodes:
                    self.ricorsione(node, [], 0)
        
        def ricorsione(self, nStart, parziale, pesoParziale):
                nodiAmm = self.cercaNodi(nStart, parziale)
                if not nodiAmm:
                    if pesoParziale > self.pesoMax:
                        self.pesoMax = pesoParziale
                        self.solBest = list(parziale)
                for nNext in nodiAmm:
                    parziale.append(nNext)
                    self.ricorsione(nNext, parziale, pesoParziale+self.grafo[nStart][nNext]['weight'])
                    parziale.pop()
                    
        def cercaNodi(self, nStart, parziale):
                result = []
                for node in self.grafo.neighbors(nStart):
                    if node not in parziale:
                        result.append(node)
                return result

CLASSE SIGHTING: Sighting

        from dataclasses import dataclass
        from datetime import datetime
        
        
        @dataclass
        class Sighting:
            id: str
            datetime: datetime
            city: str
            state: str
            country:str
            shape: str
            duration: int
            duration_hm: str
            comments: str
            date_posted: datetime
            latitude: float
            longitude: float
        
        
            def __str__(self):
                return self.country
        
            def __hash__(self):
                return hash(self.id)

CLASSE STATE: State

        import math
        from dataclasses import dataclass
        from datetime import datetime
        @dataclass
        class State:
            _id: int
            _Name: str
            _Capital: str
            _Lat: float
            _Lng: float
            _Area: float
            _Population: int
            _Neighbors: []
        
            @property
            def id(self):
                return self._id
            @property
            def lat(self):
                return self._Lat
            @property
            def lng(self):
                return self._Lng
            @property
            def Name(self):
                return self._Name
        
            def __str__(self):
                return self._Name
        
            def __hash__(self):
                return hash(self._id)

DISTANZA GEO

    def distance_HV(self, other) -> float:
        """
        Function that calculate the approximate geodesic distance between two sightings.
        :param other: another sighting.
        :return: the approximate geodesic distance in kilometers
        """
        lat1 = self._Lat * math.pi / 180
        lon1 = self._Lng * math.pi / 180
        lat2 = other._Lat * math.pi / 180
        lon2 = other._Lng * math.pi / 180
        R = 6371  # earth radius in km
        a = math.sin(0.5 * (lat2 - lat1)) ** 2 + math.cos(lat1) * math.cos(lat2) * math.sin(0.5 * (lon2 - lon1)) ** 2
        c = 2 * math.atan2(math.sqrt(a), math.sqrt(1 - a))
        return R * c
        

        
