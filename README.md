# Optimal-Resource-Allocation-Prioritizer
// ResourcePrioritizer.java// ResourcePrioritizer.java
import java.util.HashMap;
import java.util.Map;
import java.util.Comparator;

public class ResourcePrioritizer {

    // Static Base Weights
    private static final Map<String, Integer> BASE_WEIGHTS = new HashMap<>();
    static {
        BASE_WEIGHTS.put("Rare_Crystals", 100); 
        BASE_WEIGHTS.put("Standard_Metal", 50); 
        BASE_WEIGHTS.put("Energy_Cores", 150); 
        BASE_WEIGHTS.put("Basic_Supplies", 20); 
    }
    
    // Define the required quantity for high-impact strategy (e.g., securing Rank 1)
    private static final Map<String, Integer> TARGET_REQUIREMENTS = new HashMap<>();
    static {
        TARGET_REQUIREMENTS.put("Rare_Crystals", 20); // Need 20 for final weapon upgrade
        TARGET_REQUIREMENTS.put("Standard_Metal", 500);
        TARGET_REQUIREMENTS.put("Energy_Cores", 5); // Need 5 for ultimate ability usage
        TARGET_REQUIREMENTS.put("Basic_Supplies", 100);
    }
    
    // NEW: Factor to boost priority if a resource is in deficit.
    private static final double DEFICIT_BOOST_FACTOR = 1.5; 

    /**
     * Determines the resource with the highest gathering priority based on dynamic weights.
     * @param currentResources - Map of currently held resources (Name -> Quantity).
     * @return The name of the resource to prioritize.
     */
    public String getTopPriorityResource(Map<String, Integer> currentResources) {
        Map<String, Double> dynamicPriorities = new HashMap<>();
        
        for (String resource : BASE_WEIGHTS.keySet()) {
            double currentWeight = BASE_WEIGHTS.get(resource);
            int currentAmount = currentResources.getOrDefault(resource, 0);
            int requiredAmount = TARGET_REQUIREMENTS.getOrDefault(resource, 0);
            
            // Calculate Deficit: How far are we from the required amount?
            if (currentAmount < requiredAmount) {
                // Boost priority if we are below the target requirement
                double deficitRatio = (double) (requiredAmount - currentAmount) / requiredAmount;
                currentWeight += (currentWeight * deficitRatio * DEFICIT_BOOST_FACTOR);
            }
            
            dynamicPriorities.put(resource, currentWeight);
        }

        // Return the resource with the highest calculated dynamic priority
        return dynamicPriorities.entrySet().stream()
                .max(Map.Entry.comparingByValue())
                .map(Map.Entry::getKey)
                .orElse("Standard_Metal");
    }

    public static void main(String[] args) {
        ResourcePrioritizer prioritizer = new ResourcePrioritizer();
        
        // Example of current resources: low Energy Cores, adequate Crystals
        Map<String, Integer> currentSupply = new HashMap<>();
        currentSupply.put("Rare_Crystals", 18); // Close to 20 target
        currentSupply.put("Standard_Metal", 300);
        currentSupply.put("Energy_Cores", 0);   // Far from 5 target -> HIGH PRIORITY
        currentSupply.put("Basic_Supplies", 50);
        
        System.out.println("Dynamic Top Priority Resource: " + prioritizer.getTopPriorityResource(currentSupply));
    }
}
