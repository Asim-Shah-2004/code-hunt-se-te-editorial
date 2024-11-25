# Maximum Travel Content Score: Bottom-Up Approach
## Problem Statement
Given a grid of cities and days, find the maximum score Tasha can achieve by either staying in a city (earning local content points) or traveling between cities (earning journey points) over a fixed number of days.

## Solution Approach
### Bottom-Up Dynamic Programming
We solve this problem using a bottom-up dynamic programming approach where we iteratively build the solution from day 1 to day k, considering all possible states and transitions.

### Key Concepts
1. **State Definition**
   - `dp[day][city]`: Maximum score achievable on day `day` when ending in `city`

2. **Transitions**
   - Stay in current city: `dp[day+1][city] = max(dp[day][city] + stay[day][city], dp[day+1][city])`
   - Travel to another city: `dp[day+1][nextCity] = max(dp[day][city] + travel[city][nextCity], dp[day+1][nextCity])`

3. **Base Case**
   - `dp[0][city] = 0` for all cities

### Implementation
```cpp
class Solution {
public:
    /**
     * Calculates maximum possible score for Tasha's travel content
     * 
     * @param n Number of cities
     * @param k Number of days
     * @param stay Points for staying in cities[k][n]
     * @param travel Points for traveling between cities[n][n]
     * @return Maximum achievable score
     * 
     * Time Complexity: O(k * n^2)
     * Space Complexity: O(k * n)
     */
    long long maxTravelScore(int n, int k, 
                           vector<vector<int>>& stay, 
                           vector<vector<int>>& travel) {
        // DP table initialization
        long long dp[205][205] = {{0}};
        long long ans = 0;
        
        // Bottom-up calculation
        for(int day = 0; day < k; day++) {
            for(int city = 0; city < n; city++) {
                // Option 1: Stay in current city
                dp[day + 1][city] = max(dp[day][city] + stay[day][city], 
                                      dp[day + 1][city]);
                ans = max(ans, dp[day + 1][city]);
                
                // Option 2: Travel to another city
                for(int nextCity = 0; nextCity < n; nextCity++) {
                    dp[day + 1][nextCity] = max(dp[day][city] + travel[city][nextCity],
                                              dp[day + 1][nextCity]);
                    ans = max(ans, dp[day + 1][nextCity]);
                }
            }
        }
        return ans;
    }
};

// Driver code
void solve() {
    int n, k;
    cin >> n >> k;
    
    // Input stay points
    vector<vector<int>> stay(k, vector<int>(n));
    for(int i = 0; i < k; i++) {
        for(int j = 0; j < n; j++) {
            cin >> stay[i][j];
        }
    }
    
    // Input travel points
    vector<vector<int>> travel(n, vector<int>(n));
    for(int i = 0; i < n; i++) {
        for(int j = 0; j < n; j++) {
            cin >> travel[i][j];
        }
    }
    
    Solution solution;
    cout << solution.maxTravelScore(n, k, stay, travel) << endl;
}

int main() {
    solve();
    return 0;
}
```

## Complexity Analysis
- **Time Complexity**: O(k * n²)
  - For each day (k), we consider each city (n) and possible transitions to other cities (n)
  - Total: k * n * n operations
  
- **Space Complexity**: O(k * n)
  - DP table size: k days × n cities
  - Additional space for input arrays: O(k*n + n²)

## Example Test Case
```
Input:
2 3  // n = 2 cities, k = 3 days
1 2  // stay points for day 1
3 1  // stay points for day 2
2 3  // stay points for day 3
0 5  // travel points
4 0  // travel points

Output:
11   // Maximum achievable score
```

### Test Case Explanation
1. Day 1: Start in city 1, score = 2
2. Day 2: Travel to city 0, score = 2 + 4 + 3 = 9
3. Day 3: Stay in city 0, score = 9 + 2 = 11

## Common Pitfalls
1. Not initializing the DP table properly
2. Forgetting to update the maximum answer after each state transition
3. Not handling the case when staying in the same city
4. Integer overflow (use long long for large test cases)

## Optimization Tips
1. Use pre-computed arrays to store cumulative scores
2. Consider using space optimization to reduce memory usage
3. Cache frequently accessed values to improve performance

## Follow-up Questions
1. Can you modify the solution to also return the path taken?
2. How would you handle negative scores?
3. What if there are restrictions on consecutive stays in the same city?