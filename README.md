Blatte 1:

Aufgabe 1:

Variablen:

Für jede Hausposition  𝑖 ∈ { 1,2,3,4,5} 

Farbe : Farbe von Haus i.

Nation: Nationalität des Bewohners von Haus i. 

Gertränke : Getränk im Haus i.

Zigarettenmarke: Zigarettenmarke im Haus i.

Tieren:Haustier im Haus i.

V= Farbe, Nation, Gertränke,  Zigarettenmarke, Tieren.

Wertebereiche:

Farbe : {rot, Grüne, Weiß, Gelb, blau}

Nation: {Norweger, Brite, Spanier, ukrainer, Japaner}

Getränke: {Kaffee, Tee, Milch ,Orangensaft }

Zigarettenmarke: {Old-cold-Zigaretten, Kools, Chesterfield, Lucky-Strike, Parliaments }

Tier: {Hund, Schnecken, Fuchs, Pferd}

Constraints:

unäre

C9: Milch wird im mittleren Haus getrunken: Drink₃ = Milch

C10: Der Norweger wohnt im ersten Haus : Nation₁ = Norweger

Binäre:

C1: Der Engländer wohnt im roten Haus:  Nation = Brite ⟺ Color = rot

C2: Der Spanier hat einen Hund :  Nation = Spanier ⟺  Tier = Hund

C3: Kaffee wird im grünen Haus getrunken:   Getränke = Kaffee ⟺  Farbe = Grüne

C4: Der Ukrainer trinkt Tee :  Nation = Ukrainer  ⟺  Getränke = Tee.

C6: Der Raucher von Old-Gold-Zigaretten hält Schnecken als Haustiere:  Zigarettenmarke = Old-Gold-Zigaretten ⟺ Tier = Schnecken

C7: Die Zigaretten der Marke Kools werden im gelben Haus geraucht: Zigarettenmarke = Kools  ⟺ Farbe = Gelb.

C11: Der Lucky-Strike-Raucher trinkt am liebsten Orangensaft: igarettenmarke = Lucky-Strike  ⟺ Getränke = Orangensaft.

C12: Der Japaner raucht Zigaretten der Marke Parliaments: Nation = Japaner   ⟺ Zigarettenmarke = Parliaments.

-->CSP.02: Framework für Constraint Satisfaction

import copy

import random

import time


#  Variablen und Domains

houses = [1,2,3,4,5]

colors = ['rot','grün','weiß','gelb','blau']

nationalities = ['Engländer','Spanier','Ukrainer','Norweger','Japaner']

drinks = ['Kaffee','Tee','Milch','Orangensaft','Wasser']

cigarettes = ['Old-Gold','Kools','Chesterfield','Lucky-Strike','Parliaments']

pets = ['Hund','Schnecken','Fuchs','Pferd','Zebra']

variables = colors + nationalities + drinks + cigarettes + pets

domains = {v: houses[:] for v in variables}

def all_constraints(assignment):

    # 1. Engländer im roten Haus
    
    if 'Engländer' in assignment and 'rot' in assignment:
    
        if assignment['Engländer'] != assignment['rot']:
        
            return False
            
    # 2. Spanier hat Hund
    
    if 'Spanier' in assignment and 'Hund' in assignment:
    
        if assignment['Spanier'] != assignment['Hund']:
        
            return False
            
    # 3. Kaffee im grünen Haus
    
    if 'Kaffee' in assignment and 'grün' in assignment:
    
        if assignment['Kaffee'] != assignment['grün']:
        
            return False
            
    # 4. Ukrainer trinkt Tee
    
    if 'Ukrainer' in assignment and 'Tee' in assignment:
    
        if assignment['Ukrainer'] != assignment['Tee']:
        
            return False
            
    # 5. Grün rechts von Weiß
    
    if 'grün' in assignment and 'weiß' in assignment:
    
        if assignment['grün'] != assignment['weiß'] + 1:
        
            return False
            
    # 6. Old-Gold raucht Schnecken
    
    if 'Old-Gold' in assignment and 'Schnecken' in assignment:
    
        if assignment['Old-Gold'] != assignment['Schnecken']:
        
            return False
            
    # 7. Kools im gelben Haus
    
    if 'Kools' in assignment and 'gelb' in assignment:
    
        if assignment['Kools'] != assignment['gelb']:
        
            return False
            
    # 8. Milch im mittleren Haus
    
    if 'Milch' in assignment:
    
        if assignment['Milch'] != 3:
        
            return False
            
    # 9. Norweger im ersten Haus
    
    if 'Norweger' in assignment:
    
        if assignment['Norweger'] != 1:
        
            return False
            
    # 10. Chesterfield neben Fuchs
    
    if 'Chesterfield' in assignment and 'Fuchs' in assignment:
    
        if abs(assignment['Chesterfield'] - assignment['Fuchs']) != 1:
        
            return False
            
    # 11. Kools neben Pferd
    
    if 'Kools' in assignment and 'Pferd' in assignment:
    
        if abs(assignment['Kools'] - assignment['Pferd']) != 1:
        
            return False
            
    # 12. Lucky-Strike trinkt Orangensaft
    
    if 'Lucky-Strike' in assignment and 'Orangensaft' in assignment:
    
        if assignment['Lucky-Strike'] != assignment['Orangensaft']:
        
            return False
            
    # 13. Japaner raucht Parliaments
    
    if 'Japaner' in assignment and 'Parliaments' in assignment:
    
        if assignment['Japaner'] != assignment['Parliaments']:
        
            return False
            
    # 14. Norweger neben blauem Haus
    
    if 'Norweger' in assignment and 'blau' in assignment:
    
        if abs(assignment['Norweger'] - assignment['blau']) != 1:
        
            return False
            
    # 15. Alle Attribute einzigartig in Häusern
    
    for group in [colors, nationalities, drinks, cigarettes, pets]:
    
        assigned_values = [assignment[v] for v in group if v in assignment]
        
        if len(assigned_values) != len(set(assigned_values)):
        
            return False
            
    return True

# 1 BT_Search Baseline

def complete(assignment, csp):

    return len(assignment) == len(csp['variables'])

def VALUES(csp, var):

    return csp['domains'][var]

def consistent(value, var, assignment, csp):

    temp = assignment.copy()
    
    temp[var] = value
    
    return all_constraints(temp)

def INFERENCE(csp, assignment, var):

    return 'success'

def VARIABLES(csp, assignment):

    for v in csp['variables']:
    
        if v not in assignment:
        
            return v

def BT_Search(assignment, csp):

    if complete(assignment, csp):
    
        return assignment
        
    var = VARIABLES(csp, assignment)
    
    for value in VALUES(csp, var):
    
        if consistent(value, var, assignment, csp):
        
            assignment[var] = value
            
            if INFERENCE(csp, assignment, var) != 'failure':
            
                result = BT_Search(assignment, csp)
                
                if result != 'failure':
                
                    return result
                    
            del assignment[var]
            
    return 'failure'


#  MRV + Degree Heuristik

def VARIABLES_MRV(csp, assignment):

    unassigned = [v for v in csp['variables'] if v not in assignment]
    
    min_len = min(len(csp['domains'][v]) for v in unassigned)
    
    candidates = [v for v in unassigned if len(csp['domains'][v]) == min_len]
    
    if len(candidates) > 1:
    
        max_degree = -1
        
        selected = candidates[0]
        
        for v in candidates:
        
            degree = sum(1 for (x,y) in [(a,b) for a in csp['variables'] for b in csp['variables'] if a!=b] 
            
                         if (x==v and y not in assignment) or (y==v and x not in assignment))
                         
            if degree > max_degree:
            
                max_degree = degree
                
                selected = v
                
        return selected
        
    return candidates[0]

def BT_Search_MRV(assignment, csp):

    if complete(assignment, csp):
    
        return assignment
        
    var = VARIABLES_MRV(csp, assignment)
    
    for value in VALUES(csp, var):
    
        if consistent(value, var, assignment, csp):
        
            assignment[var] = value
            
            if INFERENCE(csp, assignment, var) != 'failure':
            
                result = BT_Search_MRV(assignment, csp)
                
                if result != 'failure':
                
                    return result
                    
            del assignment[var]
            
    return 'failure'

# AC-3 

def satisfies_constraint(xi, val_xi, xj, val_xj):

    # Einfach: keine zwei gleichen Attribute im Haus
    
    return val_xi != val_xj

def AC3(csp):

    queue = [(xi, xj) for xi in csp['variables'] for xj in csp['variables'] if xi != xj]
    
    domains = copy.deepcopy(csp['domains'])
    
    while queue:
    
        xi, xj = queue.pop(0)
        
        revised = False
        
        for x in domains[xi][:]:
        
            if all(not satisfies_constraint(xi, x, xj, y) for y in domains[xj]):
            
                domains[xi].remove(x)
                
                revised = True
                
        if revised:
        
            for xk in csp['variables']:
            
                if xk != xi and xk != xj:
                
                    queue.append((xk, xi))
                    
        if len(domains[xi]) == 0:
        
            return 'failure'
            
    csp['domains'] = domains
    
    return 'success'


#  Min-Conflicts Heuristik

def min_conflicts(csp, max_steps=10000):

    assignment = {v: random.choice(csp['domains'][v]) for v in csp['variables']}
    
    for step in range(max_steps):
    
        conflicted = [v for v in csp['variables'] if not consistent(assignment[v], v, {k: assignment[k] for k in csp['variables'] if k != v}, csp)]
        
        if not conflicted:
        
            return assignment
            
        var = random.choice(conflicted)
        
        best_val = min(csp['domains'][var], key=lambda val: sum(
        
            not consistent(val, var, {k: assignment[k] for k in csp['variables'] if k != var}, csp) 
            
            for k in csp['variables']
        ))
        
        assignment[var] = best_val
        
    return None


csp_base = {

    'variables': variables,
    
    'domains': domains
}


# Experimente

# 8.1 BT_Search (Basis)

start = time.time()

solution_bt = BT_Search({}, copy.deepcopy(csp_base))

end = time.time()

print("=== BT_Search ===")

print("Lösung:", solution_bt)

print("Laufzeit: {:.2f} ms\n".format((end-start)*1000))

#  BT_Search + MRV+Grad

start = time.time()

solution_mrv = BT_Search_MRV({}, copy.deepcopy(csp_base))

end = time.time()

print("=== BT_Search + MRV+Grad ===")

print("Lösung:", solution_mrv)

print("Laufzeit: {:.2f} ms\n".format((end-start)*1000))

 ->Vergleich : BT_Search lauftzeit 4.59 ms ; BT_Search + MRV+Grad Laufzeit 1176.51 ms Die Laufzeit des Basis-Algorithmus BT_Search war sehr schnell, da das Problem klein  ist und die feste Reihenfolge der Variablen zufällig günstig gewählt wurde. Im Gegensatz dazu war BT_Search mit MRV- und Gradheuristik deutlich langsamer, da die zusätzlichen Berechnungen für die Heuristiken einen hohen Aufwand verursachen, während die Variablen-Domains durch fehlende Vorpropagation kaum reduziert wurden.

# 3 AC-3 + BT_Search + MRV

csp_ac3 = copy.deepcopy(csp_base)

start = time.time()

AC3(csp_ac3)

solution_ac3 = BT_Search_MRV({}, csp_ac3)

end = time.time()

print("=== AC-3 + BT + MRV ===")

print("Lösung:", solution_ac3)

print("Laufzeit: {:.2f} ms\n".format((end-start)*1000))

 --> vergleich : Laufzeit : 691.44 ms. Etwas schneller als MRV allein, da  AC-3 reduziert manche Domains, bringt aber Vorlaufkosten.

4. Min-Conflicts

start = time.time()

solution_mc = min_conflicts(copy.deepcopy(csp_base))

end = time.time()

print("=== Min-Conflicts ===")

print("Lösung:", solution_mc)

print("Laufzeit: {:.2f} ms\n".format((end-start)*1000))

 -> Es wurde keine Lösung gefunden (Laufzeit: 5797.90 ms). Der Algorithmus Min-Conflicts ist für stark eingeschränkte CSPs wie dieses ungeeignet, da er leicht in lokalen Minima stecken bleibt und dadurch keine gültige Gesamtlösung findet.


#  Wasser- und Zebra-Fragen

if solution_bt:

    wasser_trinker = [k for k in nationalities if solution_bt[k] == solution_bt['Wasser']]
    
    zebra_besitzer = [k for k in nationalities if solution_bt[k] == solution_bt['Zebra']]
    
    print("Wer trinkt Wasser?", wasser_trinker)
    
    print("Wem gehört das Zebra?", zebra_besitzer)

# CSP.03: Kantenkonsistenz mit AC-3

graph TD
    %% Knoten (Variablen) definieren
    v1(v1)
    v2(v2)
    v3(v3)
    v4(v4)  
    
    % Constraint c1 (v1, v2): v1 + v2 = 3
    v1 -- c1: v1+v2=3 --> v2
    v2 -- c1: v2+v1=3 --> v1

    % Constraint c2 (v2, v3): v2 + v3 <= 3
    v2 -- c2: v2+v3<=3 --> v3
    v3 -- c2: v3+v2<=3 --> v2

    % Constraint c3 (v1, v3): v1 <= v3
    v1 -- c3: v1<=v3 --> v3
    v3 -- c3: v3>=v1 --> v1

    % Constraint c4 (v3, v4): v3 != v4
    v3 -- c4: v3!=v4 --> v4
    v4 -- c4: v4!=v3 --> v3

Gegeben ist ein CSP mit vier Variablen v1, v2, v3, v4, deren Domänen alle bei {0,1,2,3,4,5} starten.

1. Initialisierung

Die Warteschlange Q wird mit allen acht Bögen gefüllt:

(v1, v2),(v2, v1), (v2, v3), (v3, v2), (v1, v3), (v3, v1), (v3, v4), (v4, v3)

# Zyklus 1: Erste Reduktionen

Die stärksten Constraints werden zuerst bearbeitet:

- Bogen (v1, v2>)(v1+v2=3): Werte 4,5 in D1 haben keinen passenden Partner in D2 → entfernt.

  D1 = {0,1,2,3}. Neue Bögen zu Q: (v2,v1), (v3,v1)

- Bogen (v2, v1) (v2+v1=3): Analog, 4,5 aus D2 entfernt.

  D2 = {0,1,2,3}. Neue Bögen: (v1,v2), (v3,v2)

- Bogen (v3, v2) (v3+v2≤3): Werte 4,5 aus D3 entfernt, da keine Partner in D2 existieren.

  D3 = {0,1,2,3}. Neue Bögen: (v2,v3), (v1,v3), (v4,v3)

- Bogen (v3, v1) (v3 ≤ v1): Werte 4,5 aus D3 entfernt, keine Änderung, da D3 bereits reduziert.

Domänen nach Zyklus 1:

D1 = D2 = D3 = {0,1,2,3}, D4 = {0,1,2,3,4,5}

# Zyklus 2: Propagation und Abschluss

Die verbleibenden Bögen werden geprüft:

- (v2,v3) (v2+v3≤3): Domänen konsistent → keine Änderung
  
- (v1,v3) (v1 ≤ v3): konsistent → keine Änderung
  
- (v3,v4), (v4,v3) (v3 ≠ v4): jedes Element in D3 hat Partner in D4 → keine Änderung

Keine weiteren Reduktionen möglich, AC-3 stoppt.

# Endergebnis :

D_v1 = D_v2 = D_v3 = {0,1,2,3}, D_v4 = {0,1,2,3,4,5}

CSP mit Zuweisung α = {v1 → 2}

1. Vor Kantenkonsistenz

D1 = {2}  (zugewiesen)

D2 = D3 = D4 = {0,1,2,3,4,5}

2. Kantenkonsistenz anwenden

v1 + v2 = 3  → D2 = {1}

v1 ≤ v3      → D3 ≥ 2 → D3 = {2,3,4,5}

v2 + v3 ≤ 3  → D3 ≤ 2 → D3 = {2}

v3 ≤ v1      → D3 ≤ 2 → D3 = {2}

v3 ≠ v4      → keine Änderung → D4 = {0,1,2,3,4,5}

Nach Kantenkonsistenz:

D1 = {2}, D2 = {1}, D3 = {2}, D4 = {0,1,2,3,4,5}

3. Forward Checking

 Prüft Constraints von v1 zu unbelegten Variablen:

  v1 + v2 = 3 → D2 = {1}

  v1 ≤ v3     → D3 ≥ 2

  v2 + v3 ≤ 3 → D3 ≤ 2 → D3 = {2}

  v3 ≠ v4     → keine Änderung → D4 = {0,1,2,3,4,5}

Ergebnis Forward Checking:

D1 = {2}, D2 = {1}, D3 = {2}, D4 = {0,1,2,3,4,5}

4. Vergleich
   
Kantenkonsistenz und Forward Checking liefern hier das gleiche Ergebnis.

