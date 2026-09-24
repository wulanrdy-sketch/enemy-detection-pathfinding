Nama  : Wulan Rini Diniyanti
Nik   : 20240801403
Tugas : 1
     


# Enemy Detection and Pathfinding in Dungeon

## 1. Identifikasi Algoritma

Algoritma yang digunakan adalah:

1. **Distance Check (Euclidean Distance)** untuk mendeteksi apakah player berada dalam jangkauan enemy.
2. **A* (A-Star) Pathfinding** untuk mencari jalur dari posisi enemy menuju posisi player ketika player berada dalam jangkauan.

### Alur kerja

- Enemy mencari posisi player.
- Hitung jarak Enemy dengan Player.
- Jika jarak lebih besar dari jangkauan, Enemy tetap diam/patroli.
- Jika jarak berada dalam jangkauan, Enemy menggunakan A* untuk mencari jalur.
- Enemy bergerak mengikuti jalur menuju Player.

---

## 2. Flowchart

```text
        +----------------+
        |      MULAI     |
        +-------+--------+
                |
                v
       +-------------------+
       | Deteksi posisi    |
       | Player            |
       +---------+---------+
                 |
                 v
       +-------------------+
       | Hitung jarak      |
       | Enemy - Player    |
       +---------+---------+
                 |
                 v
          +-------------+
          | Jarak <=    |
          | Range?      |
          +------+------+
             Ya  |  Tidak
                 | 
        +--------v----+       +------------------+
        | Cari jalur  |       | Enemy patroli /  |
        | dengan A*   |       | tetap diam       |
        +--------+----+       +--------+---------+
                 |                     |
                 v                     |
        +----------------+             |
        | Enemy bergerak |             |
        | menuju Player  |             |
        +--------+-------+             |
                 |                     |
                 +----------+----------+
                            |
                            v
                       +---------+
                       | Selesai |
                       +---------+
```

---

## 3. Code Snippet

Contoh menggunakan **Python**. Kode berikut menunjukkan konsep deteksi jarak dan A* pathfinding pada grid dungeon.

```python
import math
import heapq

# Posisi enemy dan player
enemy = (1, 1)
player = (6, 5)

# Jangkauan deteksi enemy
DETECTION_RANGE = 7


def distance(a, b):
    """Menghitung jarak antara dua posisi."""
    return math.sqrt((a[0] - b[0])**2 + (a[1] - b[1])**2)


def heuristic(a, b):
    """Heuristic Manhattan Distance untuk A*."""
    return abs(a[0] - b[0]) + abs(a[1] - b[1])


def astar(grid, start, goal):
    """Mencari jalur menggunakan algoritma A*."""
    open_set = []
    heapq.heappush(open_set, (0, start))

    came_from = {}
    cost_so_far = {start: 0}

    directions = [
        (1, 0), (-1, 0),
        (0, 1), (0, -1)
    ]

    while open_set:
        _, current = heapq.heappop(open_set)

        if current == goal:
            path = []
            while current in came_from:
                path.append(current)
                current = came_from[current]

            path.append(start)
            return path[::-1]

        for dx, dy in directions:
            next_pos = (current[0] + dx, current[1] + dy)

            # Pastikan posisi masih berada di dalam grid
            if not (0 <= next_pos[1] < len(grid)):
                continue
            if not (0 <= next_pos[0] < len(grid[0])):
                continue

            # 0 = jalan, 1 = obstacle
            if grid[next_pos[1]][next_pos[0]] == 1:
                continue

            new_cost = cost_so_far[current] + 1

            if next_pos not in cost_so_far or new_cost < cost_so_far[next_pos]:
                cost_so_far[next_pos] = new_cost
                priority = new_cost + heuristic(next_pos, goal)

                heapq.heappush(open_set, (priority, next_pos))
                came_from[next_pos] = current

    return []


# Contoh dungeon
# 0 = jalan
# 1 = dinding
grid = [
    [0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 1, 1, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 1, 0, 0],
    [0, 1, 1, 0, 0, 1, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0],
]

# 1. Deteksi player
if distance(enemy, player) <= DETECTION_RANGE:

    print("Player terdeteksi!")

    # 2. Cari jalur menuju player
    path = astar(grid, enemy, player)

    if path:
        print("Jalur ditemukan:")
        print(path)

        # 3. Enemy bergerak mengikuti jalur
        for position in path[1:]:
            print("Enemy bergerak ke:", position)
    else:
        print("Jalur menuju player tidak ditemukan.")

else:
    print("Player berada di luar jangkauan.")
    print("Enemy tetap diam atau melakukan patroli.")
```

## Kesimpulan

Pada kasus ini, **Distance Check** digunakan untuk mengetahui apakah Player berada dalam jangkauan Enemy. Jika Player terdeteksi, algoritma **A*** digunakan untuk mencari jalur yang dapat dilewati menuju Player. Setelah jalur ditemukan, Enemy bergerak mengikuti jalur tersebut.


