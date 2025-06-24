# CheckboxGroup Component

## Overview
The CheckboxGroup component renders a collection of individual checkbox inputs from an options array. It manages multiple selections as an array and provides both traditional checkbox and button-style variants. The component handles individual checkbox state changes, maintains a synchronized array of selected values, and integrates with form validation systems through proper event emission.

## Basic Usage
```vue
<template>
  <div class="checkbox-group-form">
    <codex-checkbox-group
      v-model="selectedValues"
      :options="checkboxOptions"
      :dusk="'skills-selection'"
    />
  </div>
</template>

<script setup>
const selectedValues = ref([])

const checkboxOptions = [
  { value: 'javascript', displayValue: 'JavaScript' },
  { value: 'vue', displayValue: 'Vue.js' },
  { value: 'react', displayValue: 'React' },
  { value: 'node', displayValue: 'Node.js' }
]
</script>
```

## Key Features
- Multiple checkbox selection with array value management
- Traditional and button-style checkbox variants
- Dynamic option rendering from configuration arrays
- Individual checkbox state tracking and synchronization
- Flexible layout configurations for responsive design
- Custom true/false values for checkbox states
- Form integration with proper value change emission
- Testing support with Dusk attributes
- Injection-based configuration for parent communication

## Configuration Props

### Model Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `modelValue` | `Array` | `[]` | Array of currently selected values |

### Testing Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `dusk` | `String` | `''` | Browser testing identifier |

## Injected Props

The component receives configuration from parent via Vue's provide/inject:

| Injected Property | Type | Usage |
|------------------|------|-------|
| `variant` | `String` | Checkbox style (default, button) |
| `layout` | `String` | Layout size for individual checkboxes |
| `placeholder` | `String` | Placeholder for checkbox inputs |
| `name` | `String` | Base name for checkbox naming |
| `id` | `String` | Base ID for checkbox identification |
| `required` | `Boolean` | Whether checkboxes are required |
| `options` | `Array` | Array of checkbox options |
| `trueValue` | `Any` | Value when checkbox is checked |
| `falseValue` | `Any` | Value when checkbox is unchecked |

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `update:modelValue` | `Array` | Emitted when selection changes |

## Option Configuration

Options should be provided as an array of objects:

```javascript
const options = [
  { 
    value: 'option1',              // Value stored in selection array
    displayValue: 'Option 1',      // Label shown to user
    disabled: false                // Optional: disable specific option
  },
  { 
    value: 'option2', 
    displayValue: 'Option 2' 
  }
]
```

## Model Value

The component manages an array of selected checkbox values:

```vue
<template>
  <codex-checkbox-group v-model="selections" :options="options" />
</template>

<script setup>
const selections = ref([])
// selections automatically updates as user checks/unchecks options
// Contains array of selected option values: ['value1', 'value3', ...]
</script>
```

## Selection Management

The component provides internal methods for selection handling:

- `isValueSelected(value)` - Checks if a value is currently selected
- `handleCheckboxChange(value, isChecked)` - Manages adding/removing selections

```javascript
// Internal selection logic
const isValueSelected = (value) => {
  return props.modelValue.includes(value)
}

const handleCheckboxChange = (value, isChecked) => {
  if (isChecked) {
    // Add value if not present
    if (!props.modelValue.includes(value)) {
      emit('update:modelValue', [...props.modelValue, value])
    }
  } else {
    // Remove value from array
    emit('update:modelValue', props.modelValue.filter(v => v !== value))
  }
}
```

## Examples

### Skills Selection Interface
```vue
<template>
  <div class="skills-selection">
    <h3>Technical Skills Assessment</h3>
    
    <div class="skills-categories">
      <div class="skill-category">
        <h4>Programming Languages</h4>
        <p>Select the programming languages you're proficient in:</p>
        
        <codex-checkbox-group
          v-model="skills.programmingLanguages"
          :options="programmingOptions"
          :dusk="'programming-languages'"
        />
      </div>
      
      <div class="skill-category">
        <h4>Frontend Frameworks</h4>
        <p>Choose the frontend frameworks you have experience with:</p>
        
        <codex-checkbox-group
          v-model="skills.frontendFrameworks"
          :options="frontendOptions"
          :dusk="'frontend-frameworks'"
        />
      </div>
      
      <div class="skill-category">
        <h4>Backend Technologies</h4>
        <p>Select backend technologies you've worked with:</p>
        
        <codex-checkbox-group
          v-model="skills.backendTechnologies"
          :options="backendOptions"
          :dusk="'backend-technologies'"
        />
      </div>
      
      <div class="skill-category">
        <h4>Development Tools</h4>
        <p>Mark the development tools you regularly use:</p>
        
        <codex-checkbox-group
          v-model="skills.developmentTools"
          :options="toolsOptions"
          :dusk="'development-tools'"
        />
      </div>
    </div>
    
    <div class="skills-analysis" v-if="hasSelectedSkills">
      <h4>Skills Analysis</h4>
      
      <div class="analysis-dashboard">
        <div class="skill-stats">
          <div class="stat-card">
            <h5>Total Skills</h5>
            <div class="stat-value">{{ totalSkillsCount }}</div>
            <div class="stat-description">Across all categories</div>
          </div>
          
          <div class="stat-card">
            <h5>Skill Level</h5>
            <div class="stat-value">{{ skillLevel.label }}</div>
            <div class="stat-description">{{ skillLevel.description }}</div>
          </div>
          
          <div class="stat-card">
            <h5>Specialization</h5>
            <div class="stat-value">{{ primarySpecialization }}</div>
            <div class="stat-description">Your strongest area</div>
          </div>
          
          <div class="stat-card">
            <h5>Market Value</h5>
            <div class="stat-value">${{ estimatedSalaryRange.min }}k - ${{ estimatedSalaryRange.max }}k</div>
            <div class="stat-description">Estimated range</div>
          </div>
        </div>
        
        <div class="skills-breakdown">
          <h5>Skills by Category</h5>
          <div class="category-breakdown">
            <div v-for="category in skillCategories" :key="category.key" class="category-item">
              <div class="category-header">
                <h6>{{ category.name }}</h6>
                <span class="category-count">{{ skills[category.key].length }} selected</span>
              </div>
              <div class="category-skills">
                <span v-for="skill in getSelectedSkills(category.key, category.options)" 
                      :key="skill" 
                      class="skill-tag">
                  {{ skill }}
                </span>
              </div>
              <div class="category-progress">
                <div class="progress-bar">
                  <div class="progress-fill" :style="{ width: getCategoryProgress(category.key, category.options) + '%' }"></div>
                </div>
                <span class="progress-text">{{ getCategoryProgress(category.key, category.options) }}% coverage</span>
              </div>
            </div>
          </div>
        </div>
        
        <div class="skill-recommendations" v-if="skillRecommendations.length > 0">
          <h5>Skill Development Recommendations</h5>
          <div class="recommendations-list">
            <div v-for="recommendation in skillRecommendations" :key="recommendation.skill" class="recommendation-card">
              <div class="recommendation-header">
                <h6>{{ recommendation.skill }}</h6>
                <span class="recommendation-priority" :class="recommendation.priority">
                  {{ recommendation.priority }}
                </span>
              </div>
              <p class="recommendation-reason">{{ recommendation.reason }}</p>
              <div class="recommendation-benefits">
                <span v-for="benefit in recommendation.benefits" :key="benefit" class="benefit-tag">
                  {{ benefit }}
                </span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    
    <div class="skills-actions">
      <button @click="clearAllSkills" class="clear-btn">
        Clear All Selections
      </button>
      <button @click="selectCommonStack" class="preset-btn">
        Select Common Full-Stack
      </button>
      <button @click="saveSkillsProfile" :disabled="!hasMinimumSkills" class="save-btn">
        Save Skills Profile
      </button>
      <button @click="generateResume" :disabled="!hasSelectedSkills" class="resume-btn">
        Generate Skills Resume
      </button>
    </div>
  </div>
</template>

<script setup>
const skills = reactive({
  programmingLanguages: [],
  frontendFrameworks: [],
  backendTechnologies: [],
  developmentTools: []
})

const programmingOptions = [
  { value: 'javascript', displayValue: 'JavaScript' },
  { value: 'typescript', displayValue: 'TypeScript' },
  { value: 'python', displayValue: 'Python' },
  { value: 'java', displayValue: 'Java' },
  { value: 'csharp', displayValue: 'C#' },
  { value: 'php', displayValue: 'PHP' },
  { value: 'ruby', displayValue: 'Ruby' },
  { value: 'go', displayValue: 'Go' },
  { value: 'rust', displayValue: 'Rust' },
  { value: 'swift', displayValue: 'Swift' }
]

const frontendOptions = [
  { value: 'react', displayValue: 'React' },
  { value: 'vue', displayValue: 'Vue.js' },
  { value: 'angular', displayValue: 'Angular' },
  { value: 'svelte', displayValue: 'Svelte' },
  { value: 'nextjs', displayValue: 'Next.js' },
  { value: 'nuxt', displayValue: 'Nuxt.js' },
  { value: 'gatsby', displayValue: 'Gatsby' },
  { value: 'ember', displayValue: 'Ember.js' }
]

const backendOptions = [
  { value: 'nodejs', displayValue: 'Node.js' },
  { value: 'express', displayValue: 'Express.js' },
  { value: 'django', displayValue: 'Django' },
  { value: 'flask', displayValue: 'Flask' },
  { value: 'spring', displayValue: 'Spring Boot' },
  { value: 'laravel', displayValue: 'Laravel' },
  { value: 'rails', displayValue: 'Ruby on Rails' },
  { value: 'dotnet', displayValue: '.NET Core' }
]

const toolsOptions = [
  { value: 'git', displayValue: 'Git' },
  { value: 'docker', displayValue: 'Docker' },
  { value: 'kubernetes', displayValue: 'Kubernetes' },
  { value: 'aws', displayValue: 'AWS' },
  { value: 'azure', displayValue: 'Azure' },
  { value: 'gcp', displayValue: 'Google Cloud' },
  { value: 'jenkins', displayValue: 'Jenkins' },
  { value: 'webpack', displayValue: 'Webpack' },
  { value: 'babel', displayValue: 'Babel' },
  { value: 'eslint', displayValue: 'ESLint' }
]

const skillCategories = [
  { key: 'programmingLanguages', name: 'Programming Languages', options: programmingOptions },
  { key: 'frontendFrameworks', name: 'Frontend Frameworks', options: frontendOptions },
  { key: 'backendTechnologies', name: 'Backend Technologies', options: backendOptions },
  { key: 'developmentTools', name: 'Development Tools', options: toolsOptions }
]

const hasSelectedSkills = computed(() => {
  return Object.values(skills).some(skillArray => skillArray.length > 0)
})

const totalSkillsCount = computed(() => {
  return Object.values(skills).reduce((total, skillArray) => total + skillArray.length, 0)
})

const hasMinimumSkills = computed(() => {
  return totalSkillsCount.value >= 5
})

const skillLevel = computed(() => {
  const count = totalSkillsCount.value
  if (count >= 20) return { label: 'Expert', description: 'Highly skilled across multiple domains' }
  if (count >= 15) return { label: 'Senior', description: 'Strong expertise in key areas' }
  if (count >= 10) return { label: 'Mid-level', description: 'Solid foundation with growing expertise' }
  if (count >= 5) return { label: 'Junior', description: 'Good starting skillset' }
  return { label: 'Entry-level', description: 'Building foundational skills' }
})

const primarySpecialization = computed(() => {
  let maxCount = 0
  let primaryCategory = 'General'
  
  skillCategories.forEach(category => {
    const count = skills[category.key].length
    if (count > maxCount) {
      maxCount = count
      primaryCategory = category.name
    }
  })
  
  return primaryCategory
})

const estimatedSalaryRange = computed(() => {
  const baseMin = 50
  const baseMax = 80
  const skillMultiplier = Math.min(totalSkillsCount.value * 2, 50)
  
  return {
    min: baseMin + skillMultiplier,
    max: baseMax + skillMultiplier + 20
  }
})

const getSelectedSkills = (categoryKey, options) => {
  return skills[categoryKey].map(value => {
    const option = options.find(opt => opt.value === value)
    return option ? option.displayValue : value
  })
}

const getCategoryProgress = (categoryKey, options) => {
  const selectedCount = skills[categoryKey].length
  const totalCount = options.length
  return Math.round((selectedCount / totalCount) * 100)
}

const skillRecommendations = computed(() => {
  const recommendations = []
  
  // JavaScript developers should learn TypeScript
  if (skills.programmingLanguages.includes('javascript') && !skills.programmingLanguages.includes('typescript')) {
    recommendations.push({
      skill: 'TypeScript',
      priority: 'high',
      reason: 'Essential for scalable JavaScript development and better code quality',
      benefits: ['Type Safety', 'Better IDE Support', 'Enterprise Ready']
    })
  }
  
  // React developers should consider Next.js
  if (skills.frontendFrameworks.includes('react') && !skills.frontendFrameworks.includes('nextjs')) {
    recommendations.push({
      skill: 'Next.js',
      priority: 'medium',
      reason: 'Popular React framework for production applications',
      benefits: ['SSR/SSG', 'Better Performance', 'Deployment Ready']
    })
  }
  
  // Backend developers should learn Docker
  if (skills.backendTechnologies.length > 0 && !skills.developmentTools.includes('docker')) {
    recommendations.push({
      skill: 'Docker',
      priority: 'high',
      reason: 'Essential for modern deployment and development workflows',
      benefits: ['Containerization', 'DevOps Skills', 'Scalability']
    })
  }
  
  return recommendations.slice(0, 3) // Limit to 3 recommendations
})

const clearAllSkills = () => {
  Object.keys(skills).forEach(key => {
    skills[key] = []
  })
  toast.info('All skills cleared')
}

const selectCommonStack = () => {
  skills.programmingLanguages = ['javascript', 'typescript']
  skills.frontendFrameworks = ['react', 'vue']
  skills.backendTechnologies = ['nodejs', 'express']
  skills.developmentTools = ['git', 'docker', 'aws']
  toast.success('Common full-stack skills selected')
}

const saveSkillsProfile = async () => {
  try {
    await saveUserSkills({
      skills,
      totalCount: totalSkillsCount.value,
      level: skillLevel.value.label,
      specialization: primarySpecialization.value
    })
    
    toast.success('Skills profile saved successfully!')
  } catch (error) {
    toast.error('Failed to save skills profile')
  }
}

const generateResume = () => {
  const skillsData = encodeURIComponent(JSON.stringify(skills))
  router.push(`/resume/generate?skills=${skillsData}`)
}
</script>
```

### Feature Selection Dashboard
```vue
<template>
  <div class="feature-selection">
    <h3>Product Feature Selection</h3>
    
    <div class="feature-categories">
      <div class="feature-tier">
        <h4>Core Features</h4>
        <p class="tier-description">Essential features included in all plans</p>
        
        <div class="core-features-list">
          <div v-for="feature in coreFeatures" :key="feature.value" class="core-feature-item">
            <i class="ri-check-line"></i>
            <span>{{ feature.displayValue }}</span>
            <span class="included-badge">Included</span>
          </div>
        </div>
      </div>
      
      <div class="feature-tier">
        <h4>Premium Add-ons</h4>
        <p class="tier-description">Select additional features to enhance your plan</p>
        
        <codex-checkbox-group
          v-model="selectedFeatures.premium"
          :options="premiumFeatures"
          :dusk="'premium-features'"
        />
      </div>
      
      <div class="feature-tier">
        <h4>Enterprise Features</h4>
        <p class="tier-description">Advanced features for large organizations</p>
        
        <codex-checkbox-group
          v-model="selectedFeatures.enterprise"
          :options="enterpriseFeatures"
          :dusk="'enterprise-features'"
        />
      </div>
      
      <div class="feature-tier">
        <h4>Integration Options</h4>
        <p class="tier-description">Connect with your existing tools</p>
        
        <codex-checkbox-group
          v-model="selectedFeatures.integrations"
          :options="integrationFeatures"
          :dusk="'integration-features'"
        />
      </div>
    </div>
    
    <div class="feature-summary" v-if="hasSelectedAddons">
      <h4>Your Plan Summary</h4>
      
      <div class="plan-overview">
        <div class="plan-details">
          <h5>Selected Features</h5>
          
          <div class="feature-sections">
            <div class="feature-section">
              <h6>Core Features (Included)</h6>
              <div class="feature-tags">
                <span v-for="feature in coreFeatures" :key="feature.value" class="feature-tag core">
                  {{ feature.displayValue }}
                </span>
              </div>
            </div>
            
            <div v-if="selectedFeatures.premium.length > 0" class="feature-section">
              <h6>Premium Add-ons</h6>
              <div class="feature-tags">
                <span v-for="feature in getSelectedFeatureLabels('premium', premiumFeatures)" 
                      :key="feature" 
                      class="feature-tag premium">
                  {{ feature }}
                </span>
              </div>
            </div>
            
            <div v-if="selectedFeatures.enterprise.length > 0" class="feature-section">
              <h6>Enterprise Features</h6>
              <div class="feature-tags">
                <span v-for="feature in getSelectedFeatureLabels('enterprise', enterpriseFeatures)" 
                      :key="feature" 
                      class="feature-tag enterprise">
                  {{ feature }}
                </span>
              </div>
            </div>
            
            <div v-if="selectedFeatures.integrations.length > 0" class="feature-section">
              <h6>Integrations</h6>
              <div class="feature-tags">
                <span v-for="feature in getSelectedFeatureLabels('integrations', integrationFeatures)" 
                      :key="feature" 
                      class="feature-tag integration">
                  {{ feature }}
                </span>
              </div>
            </div>
          </div>
        </div>
        
        <div class="pricing-summary">
          <h5>Pricing Breakdown</h5>
          
          <div class="price-items">
            <div class="price-item base">
              <span class="price-label">Base Plan</span>
              <span class="price-value">${{ basePlanPrice }}/month</span>
            </div>
            
            <div v-if="premiumAddonsCost > 0" class="price-item">
              <span class="price-label">Premium Add-ons ({{ selectedFeatures.premium.length }})</span>
              <span class="price-value">+${{ premiumAddonsCost }}/month</span>
            </div>
            
            <div v-if="enterpriseAddonsCost > 0" class="price-item">
              <span class="price-label">Enterprise Features ({{ selectedFeatures.enterprise.length }})</span>
              <span class="price-value">+${{ enterpriseAddonsCost }}/month</span>
            </div>
            
            <div v-if="integrationsCost > 0" class="price-item">
              <span class="price-label">Integrations ({{ selectedFeatures.integrations.length }})</span>
              <span class="price-value">+${{ integrationsCost }}/month</span>
            </div>
            
            <div class="price-item total">
              <span class="price-label">Total Monthly Cost</span>
              <span class="price-value">${{ totalMonthlyCost }}/month</span>
            </div>
            
            <div class="price-item annual" v-if="showAnnualDiscount">
              <span class="price-label">Annual Cost (20% discount)</span>
              <span class="price-value">${{ annualCost }}/year</span>
              <span class="savings">Save ${{ annualSavings }}!</span>
            </div>
          </div>
          
          <div class="plan-tier-badge">
            <span class="tier-name">{{ currentPlanTier }}</span>
            <span class="tier-description">{{ currentPlanDescription }}</span>
          </div>
        </div>
      </div>
      
      <div class="feature-recommendations" v-if="featureRecommendations.length > 0">
        <h5>Recommended Add-ons</h5>
        <div class="recommendations">
          <div v-for="recommendation in featureRecommendations" :key="recommendation.feature" class="recommendation-item">
            <div class="recommendation-content">
              <h6>{{ recommendation.feature }}</h6>
              <p>{{ recommendation.reason }}</p>
              <div class="recommendation-benefits">
                <span v-for="benefit in recommendation.benefits" :key="benefit" class="benefit">
                  {{ benefit }}
                </span>
              </div>
            </div>
            <button @click="addRecommendedFeature(recommendation)" class="add-feature-btn">
              Add (+${{ recommendation.price }}/mo)
            </button>
          </div>
        </div>
      </div>
    </div>
    
    <div class="feature-actions">
      <button @click="resetFeatures" class="reset-btn">
        Reset to Core Plan
      </button>
      <button @click="selectPopularPlan" class="preset-btn">
        Select Popular Plan
      </button>
      <button @click="upgradeNow" :disabled="!hasSelectedAddons" class="upgrade-btn">
        Upgrade Plan - ${{ totalMonthlyCost }}/month
      </button>
    </div>
  </div>
</template>

<script setup>
const selectedFeatures = reactive({
  premium: [],
  enterprise: [],
  integrations: []
})

const coreFeatures = [
  { value: 'basic_dashboard', displayValue: 'Basic Dashboard' },
  { value: 'user_management', displayValue: 'User Management' },
  { value: 'basic_reporting', displayValue: 'Basic Reporting' },
  { value: 'email_support', displayValue: 'Email Support' },
  { value: 'mobile_app', displayValue: 'Mobile App Access' }
]

const premiumFeatures = [
  { value: 'advanced_analytics', displayValue: 'Advanced Analytics ($15/mo)', price: 15 },
  { value: 'custom_branding', displayValue: 'Custom Branding ($10/mo)', price: 10 },
  { value: 'priority_support', displayValue: 'Priority Support ($20/mo)', price: 20 },
  { value: 'advanced_workflows', displayValue: 'Advanced Workflows ($25/mo)', price: 25 },
  { value: 'api_access', displayValue: 'API Access ($12/mo)', price: 12 }
]

const enterpriseFeatures = [
  { value: 'sso_integration', displayValue: 'SSO Integration ($50/mo)', price: 50 },
  { value: 'advanced_security', displayValue: 'Advanced Security ($40/mo)', price: 40 },
  { value: 'dedicated_support', displayValue: 'Dedicated Support ($100/mo)', price: 100 },
  { value: 'compliance_tools', displayValue: 'Compliance Tools ($30/mo)', price: 30 },
  { value: 'custom_development', displayValue: 'Custom Development ($200/mo)', price: 200 }
]

const integrationFeatures = [
  { value: 'salesforce', displayValue: 'Salesforce Integration ($25/mo)', price: 25 },
  { value: 'slack', displayValue: 'Slack Integration ($8/mo)', price: 8 },
  { value: 'google_workspace', displayValue: 'Google Workspace ($15/mo)', price: 15 },
  { value: 'microsoft_365', displayValue: 'Microsoft 365 ($15/mo)', price: 15 },
  { value: 'zapier', displayValue: 'Zapier Integration ($12/mo)', price: 12 }
]

const basePlanPrice = 49

const hasSelectedAddons = computed(() => {
  return Object.values(selectedFeatures).some(featureArray => featureArray.length > 0)
})

const getFeatureCost = (categoryKey, featuresArray) => {
  return selectedFeatures[categoryKey].reduce((total, featureValue) => {
    const feature = featuresArray.find(f => f.value === featureValue)
    return total + (feature ? feature.price : 0)
  }, 0)
}

const premiumAddonsCost = computed(() => getFeatureCost('premium', premiumFeatures))
const enterpriseAddonsCost = computed(() => getFeatureCost('enterprise', enterpriseFeatures))
const integrationsCost = computed(() => getFeatureCost('integrations', integrationFeatures))

const totalMonthlyCost = computed(() => {
  return basePlanPrice + premiumAddonsCost.value + enterpriseAddonsCost.value + integrationsCost.value
})

const annualCost = computed(() => Math.round(totalMonthlyCost.value * 12 * 0.8))
const annualSavings = computed(() => (totalMonthlyCost.value * 12) - annualCost.value)
const showAnnualDiscount = computed(() => totalMonthlyCost.value > basePlanPrice)

const currentPlanTier = computed(() => {
  if (enterpriseAddonsCost.value > 0) return 'Enterprise'
  if (premiumAddonsCost.value > 0) return 'Professional'
  return 'Starter'
})

const currentPlanDescription = computed(() => {
  const tierDescriptions = {
    'Starter': 'Perfect for small teams getting started',
    'Professional': 'Great for growing businesses',
    'Enterprise': 'Designed for large organizations'
  }
  return tierDescriptions[currentPlanTier.value]
})

const getSelectedFeatureLabels = (categoryKey, featuresArray) => {
  return selectedFeatures[categoryKey].map(value => {
    const feature = featuresArray.find(f => f.value === value)
    return feature ? feature.displayValue.replace(/ \(\$.*\)/, '') : value
  })
}

const featureRecommendations = computed(() => {
  const recommendations = []
  
  // If they have advanced analytics, recommend API access
  if (selectedFeatures.premium.includes('advanced_analytics') && !selectedFeatures.premium.includes('api_access')) {
    recommendations.push({
      feature: 'API Access',
      reason: 'Perfect complement to Advanced Analytics for custom integrations',
      benefits: ['Custom Dashboards', 'Data Export', 'Third-party Tools'],
      price: 12,
      category: 'premium',
      value: 'api_access'
    })
  }
  
  // If they have SSO, recommend advanced security
  if (selectedFeatures.enterprise.includes('sso_integration') && !selectedFeatures.enterprise.includes('advanced_security')) {
    recommendations.push({
      feature: 'Advanced Security',
      reason: 'Essential security features to complement SSO integration',
      benefits: ['Enhanced Encryption', 'Audit Logs', 'Security Monitoring'],
      price: 40,
      category: 'enterprise',
      value: 'advanced_security'
    })
  }
  
  return recommendations.slice(0, 2)
})

const addRecommendedFeature = (recommendation) => {
  if (!selectedFeatures[recommendation.category].includes(recommendation.value)) {
    selectedFeatures[recommendation.category].push(recommendation.value)
    toast.success(`${recommendation.feature} added to your plan!`)
  }
}

const resetFeatures = () => {
  Object.keys(selectedFeatures).forEach(key => {
    selectedFeatures[key] = []
  })
  toast.info('Reset to core plan')
}

const selectPopularPlan = () => {
  selectedFeatures.premium = ['advanced_analytics', 'priority_support', 'api_access']
  selectedFeatures.integrations = ['slack', 'google_workspace']
  toast.success('Popular plan features selected!')
}

const upgradeNow = async () => {
  try {
    await upgradePlan({
      selectedFeatures,
      totalCost: totalMonthlyCost.value,
      planTier: currentPlanTier.value
    })
    
    toast.success('Plan upgraded successfully!')
    router.push('/dashboard')
  } catch (error) {
    toast.error('Failed to upgrade plan')
  }
}
</script>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-checkbox-container` | Container for default checkbox items |
| `_c-checkbox-button` | Container for button-style checkboxes |
| `_c-form-field--auto` | Auto-sized layout |
| `_c-form-field--quarter` | Quarter width layout (25%) |
| `_c-form-field--third` | Third width layout (33%) |
| `_c-form-field--half` | Half width layout (50%) |
| `_c-form-field--full` | Full width layout (100%) |
| `_c-label-container` | Container for label element |

## Best Practices

### Selection Management
- Efficiently handle array operations for adding/removing selections
- Prevent duplicate selections in the array
- Maintain consistent selection state across renders
- Use proper key management for dynamic option lists
- Handle edge cases like empty arrays gracefully

### User Experience
- Provide clear visual feedback for selected states
- Group related checkboxes logically
- Show selection counts and summaries when useful
- Allow bulk selection/deselection operations when appropriate
- Use button variant for enhanced visual appeal

### Performance
- Optimize array operations for large option sets
- Use efficient comparison methods for selection checks
- Debounce selection changes if needed for performance
- Consider virtual scrolling for very large checkbox groups
- Minimize unnecessary re-renders during selection changes

### Accessibility
- Ensure proper checkbox labeling and association
- Support keyboard navigation between checkboxes
- Use appropriate ARIA attributes for checkbox groups
- Test with screen readers and assistive technologies
- Provide clear instructions for selection requirements

### Data Handling
- Validate selected values against available options
- Handle option updates without losing user selections
- Store selections consistently as arrays of values
- Consider data normalization for consistent storage
- Provide proper type checking for selection values

### Integration
- Emit proper update events for parent components
- Handle injection-based configuration correctly
- Integrate smoothly with form validation systems
- Support testing through proper Dusk attributes
- Maintain compatibility with different layout systems

## Component Registration
```javascript
// Global registration
app.component('CodexCheckboxGroup', CheckboxGroup)

// Local registration  
import CheckboxGroup from '@/components/fields/CheckboxGroup.vue'

export default {
  components: {
    CodexCheckboxGroup: CheckboxGroup
  }
}
``` 