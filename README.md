# Optimal-Resource-Allocation-Prioritizer
// ResourcePrioritizer.java
import java.util.HashMap;
import java.util.Map;
import java.util.Comparator;

public class ResourcePrioritizer {

    // Weights: Higher weight means higher priority for gathering.
    private static final Map<String, Integer> RESOURCE_WEIGHTS = new HashMap<>();
    static {
        RESOURCE_WEIGHTS.put("Rare_Crystals", 100); // Crucial for late-game upgrades
        RESOURCE_WEIGHTS.put("Standard_Metal", 50); // General construction/repair
        RESOURCE_WEIGHTS.put("Energy_Cores", 150); // Essential for ultimate abilities/movement
        RESOURCE_WEIGHTS.put("Basic_Supplies", 20); // Low priority, passive income
    }

    /**
     * Determines the resource with the highest gathering priority based on static weights.
     * @return The name of the resource to prioritize.
     */
    public String getTopPriorityResource() {
        return RESOURCE_WEIGHTS.entrySet().stream()
                .max(Map.Entry.comparingByValue())
                .map(Map.Entry::getKey)
                .orElse("Standard_Metal"); // Default if map is empty
    }

    public static void main(String[] args) {
        ResourcePrioritizer prioritizer = new ResourcePrioritizer();
        System.out.println("Current Top Priority Resource: " + prioritizer.getTopPriorityResource());
    }
}
