# 🚀 Comprehensive Release System Implementation Tasks

## 📋 OVERVIEW

This document provides a comprehensive task breakdown for implementing the Release System with two-point slider functionality, tracking both completed and pending tasks.

**Current Status**: 🔄 IN PROGRESS
**Priority**: 🔴 HIGH

## 📁 FILE SYSTEM STRUCTURE

```
projects/
└── gevis/
    ├── dataset/
    │   ├── animal/
    │   │   ├── train/
    │   │   ├── val/
    │   │   └── test/
    ├── unassigned/
    ├── annotating/
    ├── augmented/
    │   ├── train/
    │   ├── val/
    │   └── test/
    └── release/
        └── v1_brightness_yolo.zip
```

## 🔄 RELEASE SYSTEM WORKFLOW - SMART & MINIMAL TRANSFORMATION STRATEGY

### **🎯 CORE WORKFLOW:**
1. **User selects transformations** with parameter ranges (min/max values)
2. **System applies Smart & Minimal Strategy** for efficient augmentation
3. **User configures release** (name, images per original, export format)
4. **System generates augmented images** using the minimal strategy
5. **Annotations are updated** to match transformations
6. **Export system packages** the augmented dataset in selected format
7. **User downloads** the final release package

### **🧠 SMART & MINIMAL TRANSFORMATION STRATEGY:**

**🎯 Goal**: Efficiently augment each image with high diversity, using only meaningful points, without bloating the dataset.

#### **🔧 STRATEGY LOGIC:**
For `images_per_original = N`, use this sequence:

| Step | Description | Example |
|------|-------------|---------|
| 1️⃣ | **Original Image** | `(brightness=1.0, contrast=1.0, rotation=0°)` |
| 2️⃣ | **Max Value for Tool 1** (if enabled) | `brightness = 1.2` |
| 3️⃣ | **Max Value for Tool 2** | `contrast = 1.2` |
| 4️⃣ | **Max Value for Tool 3** | `rotation = +15°` |
| 5️⃣ | **Min Value for Tool 1** | `brightness = 0.8` |
| 6️⃣ | **Min Value for Tool 2** | `contrast = 0.8` |
| 🔁 | **(Optional) Random/Combo effects** only if `N > tool_count × 2 + 1` | `(brightness + rotation)` |

#### **✅ CORE PRINCIPLES:**
- **Original = Midpoint** → No need to repeat 1.0 or 0° values
- **Max/Min = Full spectrum coverage** → See extremes of each tool
- **One transformation per image** → Clean, interpretable outputs
- **No combination explosion** → Avoid clutter and complexity
- **Predictable count** → For N enabled tools: `1 original + N max + N min = 2N+1 images minimum`

#### **🧪 PRACTICAL EXAMPLES:**

##### **Example A: 3 tools (Brightness, Contrast, Rotation), images_per_original = 6**
| Image | Applied Transformation |
|-------|----------------------|
| 1 | **Original** `(brightness=1.0, contrast=1.0, rotation=0°)` |
| 2 | **ONLY** `brightness = 1.2` (max) |
| 3 | **ONLY** `contrast = 1.2` (max) |
| 4 | **ONLY** `rotation = +15°` (max) |
| 5 | **ONLY** `brightness = 0.8` (min) |
| 6 | **ONLY** `contrast = 0.8` (min) |

##### **Example B: 2 tools (Hue, Gamma), images_per_original = 4**
| Image | Applied Transformation |
|-------|----------------------|
| 1 | **Original** `(hue=0°, gamma=1.0)` |
| 2 | **ONLY** `hue = +90°` (max) |
| 3 | **ONLY** `gamma = 2.5` (max) |
| 4 | **ONLY** `hue = -90°` (min) |

##### **Example C: 4 tools, images_per_original = 12 (with optional combos)**
| Image | Applied Transformation |
|-------|----------------------|
| 1-9 | **Basic set** (1 original + 4 max + 4 min = 9 images) |
| 10 | **Combo**: `brightness=1.2 + contrast=0.8` |
| 11 | **Combo**: `rotation=15° + hue=45°` |
| 12 | **Random combo** from remaining possibilities |

### **🎯 WHY THIS APPROACH IS BRILLIANT:**

#### **🔍 BENEFITS:**
1. **🎯 Perfect Coverage**: See each tool's full range (min to max)
2. **🧹 Clean Results**: One transformation per image = easy interpretation
3. **⚡ Efficient**: No wasted combinations or redundant variations
4. **📊 Predictable**: User knows exactly what they'll get
5. **🔍 Individual Understanding**: See pure effect of each tool
6. **📈 Scalable**: Works with any number of tools
7. **🚫 No Bloat**: Avoids exponential combination explosion

---

## 🚨 **CRITICAL IMPLEMENTATION STATUS & TESTING REQUIREMENTS**

### **⚠️ CURRENT REALITY CHECK:**

**COMPLETED WORK:**
- ✅ Backend server startup fixed and working
- ✅ File path issues resolved (cross-platform compatibility)
- ✅ Database migrations completed successfully
- ✅ Transformation preview API functional
- ✅ Documentation updated with Smart & Minimal Strategy

**🔴 MAJOR ISSUES DISCOVERED:**
1. **Frontend UI Mismatch**: Still showing old single sliders instead of new range sliders
2. **Data Format Inconsistency**: Database saving old single-value format `{"adjustment": -50, "enabled": true}` instead of range format
3. **UI Deployment Issues**: Frontend code updates not properly reflected in browser
4. **Range Slider Implementation**: Need to verify if range slider components are actually deployed
5. **Dynamic Limits**: "Images per Original" calculation not implemented based on tool count

### **🧪 URGENT TESTING REQUIREMENTS:**

#### **🔍 FRONTEND TESTING NEEDED:**
- [ ] **Verify range slider UI** is actually displaying (not single sliders)
- [ ] **Test range value saving** - should save `{"min": 0.8, "max": 1.2}` format
- [ ] **Check transformation preview** with range values
- [ ] **Validate images_per_original** dynamic calculation
- [ ] **Test browser cache issues** - clear cache and reload

#### **🔧 BACKEND TESTING NEEDED:**
- [ ] **Test Smart & Minimal Strategy** implementation
- [ ] **Verify transformation application** follows min/max/original pattern
- [ ] **Check annotation updates** work correctly with transformations
- [ ] **Test release generation** end-to-end workflow
- [ ] **Validate export functionality** with augmented images

#### **🔄 INTEGRATION TESTING NEEDED:**
- [ ] **Frontend ↔ Backend** data flow verification
- [ ] **Database schema** compatibility with range format
- [ ] **File path handling** across different environments
- [ ] **Error handling** for invalid ranges or missing tools
- [ ] **Performance testing** with multiple tools and large image sets

### **🎯 NEXT STEPS FOR IMPLEMENTATION:**

1. **🔍 IMMEDIATE**: Test current frontend UI state - verify what's actually deployed
2. **🔧 PRIORITY**: Implement Smart & Minimal Strategy in backend `schema.py`
3. **🎨 FRONTEND**: Fix range slider UI if not properly deployed
4. **🧪 TESTING**: Comprehensive end-to-end testing of transformation workflow
5. **📊 VALIDATION**: Verify all 18 transformation tools work with new strategy

### **📋 TESTING CHECKLIST:**
- [ ] Frontend displays range sliders (not single sliders)
- [ ] Range values save in correct format `{"min": X, "max": Y}`
- [ ] Smart & Minimal Strategy generates correct image count
- [ ] Transformations apply in correct order (original → max → min → combos)
- [ ] Image preview works with range-based transformations
- [ ] Export system packages augmented images correctly
- [ ] All 18 transformation tools function properly
- [ ] Cross-platform compatibility maintained
- [ ] Error handling works for edge cases
- [ ] Performance acceptable with multiple tools enabled

**🚨 CRITICAL**: Need to test and fix these issues before proceeding with full implementation!

## 📋 RELEASE CONFIG SYSTEM FILES

**Frontend Files:**
1. `/workspace/sy-app-1/frontend/src/components/project-workspace/ReleaseSection/IndividualTransformationControl.jsx`
2. `/workspace/sy-app-1/frontend/src/components/project-workspace/ReleaseSection/TransformationModal.jsx`
3. `/workspace/sy-app-1/frontend/src/components/project-workspace/ReleaseSection/releaseconfigpanel.jsx`
4. `/workspace/sy-app-1/frontend/src/components/project-workspace/ReleaseSection/ReleaseSection.jsx`
5. `/workspace/sy-app-1/frontend/src/components/project-workspace/ReleaseSection/TransformationCard.jsx`
6. `/workspace/sy-app-1/frontend/src/components/project-workspace/ReleaseSection/TransformationSection.jsx`
7. `/workspace/sy-app-1/frontend/src/components/project-workspace/ReleaseSection/ExportOptionsModal.jsx` *(for export functionality)*

**Backend Files:**
1. `/workspace/sy-app-1/backend/api/routes/releases.py`
2. `/workspace/sy-app-1/backend/api/routes/augmentation.py`
3. `/workspace/sy-app-1/backend/api/routes/transformation_preview.py`
4. `/workspace/sy-app-1/backend/database/models.py`
5. `/workspace/sy-app-1/backend/database/migrations.py`
6. `/workspace/sy-app-1/backend/utils/augmentation_utils.py`
7. `/workspace/sy-app-1/backend/utils/image_transformer.py`
8. `/workspace/sy-app-1/backend/release.py` *(in main backend folder - may need reorganization)*
9. `/workspace/sy-app-1/backend/schema.py` *(in main backend folder - may need reorganization)*

> **Note**: Files #8 and #9 are currently in the main backend folder and may need to be moved to appropriate subdirectories during future refactoring.

## 🔍 EXISTING COMPONENTS ANALYSIS

### ✅ COMPLETED TASKS

#### 1. Enhanced Export System Implementation
**Status**: ✅ COMPLETE
**Files**: 
- `/workspace/sy-app-1/backend/api/routes/enhanced_export.py`

**What was implemented:**
- 5 core export formats:
  - `yolo_detection` - YOLO format for object detection
  - `yolo_segmentation` - YOLO format for segmentation
  - `coco` - COCO JSON format
  - `pascal_voc` - Pascal VOC XML format
  - `csv` - CSV format for data analysis
- API endpoints for export and download
- Proper folder structure for exported files

**Verification Needed:**
- Test with real data to ensure all formats work correctly
- Verify format selection works with backend API
- Ensure export downloads work with new format names

#### 2. UI Export Options Update
**Status**: ✅ COMPLETE
**Files**:
- `/workspace/sy-app-1/frontend/src/components/project-workspace/ReleaseSection/ExportOptionsModal.jsx`
- `/workspace/sy-app-1/frontend/src/components/project-workspace/ReleaseSection/releaseconfigpanel.jsx`

**What was implemented:**
- Updated task types to only show Object Detection and Instance Segmentation
- Removed unsupported formats
- Added 5 core export formats matching backend
- Updated format descriptions and default values

**Verification Needed:**
- Verify UI correctly displays only supported formats
- Check that format selection works correctly
- Test with different task types

#### 3. Transformation Combination Count System
**Status**: ✅ COMPLETE
**Files**:
- `/workspace/sy-app-1/backend/database/add_transformation_combination_count_migration.py`
- `/workspace/sy-app-1/backend/api/routes/releases.py`
- `/workspace/sy-app-1/frontend/src/components/project-workspace/ReleaseSection/releaseconfigpanel.jsx`

**What was implemented:**
- Added `transformation_combination_count` column to database
- Created migration script
- Enhanced API endpoints to return combination counts
- Updated "Images per Original" input to use dynamic maximum

**Verification Needed:**
- Verify combination count calculation is correct
- Test with different transformation configurations
- Check that UI correctly limits "Images per Original" input

#### 4. Database Schema Updates and Cleanup

**Status**: ✅ COMPLETED  
**Files**:
- `/workspace/sy-app-1/backend/database/models.py`
- `/workspace/sy-app-1/backend/database/operations.py`
- `/workspace/sy-app-1/backend/database/migrations.py`
- `/workspace/sy-app-1/backend/api/routes/augmentation.py`
- `/workspace/sy-app-1/backend/api/routes/image_transformations.py`

**What was implemented:**
- Added `parameter_ranges` and `range_enabled_params` columns to ImageTransformation table
- Updated Pydantic models (TransformationCreate, TransformationUpdate, TransformationResponse)
- Successfully migrated existing database with new columns
- Removed unused DataAugmentation and ExportJob table models
- Cleaned up corresponding CRUD operations and imports
- Added migration to drop unused tables from database
- Marked deprecated augmentation API endpoints with 410 status codes
- Preserved working augmentation endpoints for legacy support

**System Architecture Clarified:**
- **ImageTransformation table**: Used for transformation configurations with parameter ranges
- **Release table**: Used for release management and export
- **image-transformations API**: Main API for transformation CRUD operations
- **augmentation API**: Only used for getAvailableTransformations() (legacy support)

#### 5. Two-Point Slider UI Implementation

**Status**: ✅ COMPLETED  
**Files**:
- `/workspace/sy-app-1/frontend/src/components/project-workspace/ReleaseSection/IndividualTransformationControl.jsx`
- `/workspace/sy-app-1/frontend/src/components/project-workspace/ReleaseSection/TransformationModal.jsx`

**What was implemented:**
- **Sub-task 1.1**: ✅ Create RangeSlider Component - Implemented dual-handle slider component with range support
- **Sub-task 1.2**: ✅ Range Parameter State Management - Added parameterRanges state and range handling logic
- **Sub-task 1.3**: ✅ Toggle Switch Integration - Added toggle switches for enabling/disabling range mode per parameter
- **Sub-task 1.4**: ✅ Combination Count Display - Added real-time combination count calculation and display

**Key Features Implemented:**
- Dual-handle range sliders for all transformation parameters
- Toggle switches to enable/disable range mode per parameter
- Special handling for brightness/contrast parameters as +/- percentages from normal
- Real-time combination count calculation with practical limits (max 1000 combinations)
- Visual feedback for excessive combinations with warning messages
- Proper state management for both single values and parameter ranges

## ❌ PENDING TASKS

### 🔹 TASK 1: TWO-POINT SLIDER UI IMPLEMENTATION

**Priority**: 🔴 HIGH  
**Status**: ✅ COMPLETED  
**Completed Time**: 4-5 hours  
**Files to Modify**:
- `/workspace/sy-app-1/frontend/src/components/project-workspace/ReleaseSection/IndividualTransformationControl.jsx`
- `/workspace/sy-app-1/frontend/src/components/project-workspace/ReleaseSection/TransformationModal.jsx`

#### 📋 Sub-Tasks:

##### 1.1 Create RangeSlider Component
- [x] **Implement dual-handle slider component**
  ```jsx
  // In IndividualTransformationControl.jsx
  const renderRangeSlider = (paramKey, paramDef) => {
    // Get range values with appropriate defaults
    const [minValue, maxValue] = parameterRanges[paramKey] || 
      getDefaultRange(paramKey, parameters[paramKey]);
    
    // Get appropriate unit label
    const unitLabel = getUnitLabel(paramKey, paramDef);
    
    // For brightness and contrast, show +/- values relative to 1.0
    const showRelativeValues = (paramKey === 'brightness' || paramKey === 'contrast');
    
    // Format value for display
    const formatValue = (val) => {
      if (showRelativeValues) {
        // Show as +/- percentage from normal
        const relativeVal = ((val - 1.0) * 100).toFixed(0);
        return relativeVal > 0 ? `+${relativeVal}%` : `${relativeVal}%`;
      }
      
      // For rotation, just show the degrees
      if (paramKey === 'rotation') {
        return `${val}°`;
      }
      
      // For scale, show as percentage
      if (paramKey === 'scale') {
        return `${val}%`;
      }
      
      // Default formatting
      return val.toFixed(2);
    };
    
    return (
      <div style={{ marginBottom: 12 }}>
        <div style={{ 
          display: 'flex', 
          justifyContent: 'space-between', 
          alignItems: 'center',
          marginBottom: 4
        }}>
          <span style={{ fontSize: '12px', fontWeight: 500 }}>
            {paramKey.charAt(0).toUpperCase() + paramKey.slice(1)}
            <span style={{ fontSize: '10px', color: '#999', marginLeft: 4 }}>
              {unitLabel}
            </span>
          </span>
          <Space>
            <Tooltip title={`Min: ${formatValue(minValue)}`}>
              <InputNumber
                size="small"
                value={minValue}
                min={paramDef.min}
                max={maxValue}
                step={paramDef.step || 0.1}
                onChange={(val) => handleRangeChange(paramKey, [val, maxValue])}
                disabled={!enabled}
                style={{ width: 60 }}
              />
            </Tooltip>
            <span>-</span>
            <Tooltip title={`Max: ${formatValue(maxValue)}`}>
              <InputNumber
                size="small"
                value={maxValue}
                min={minValue}
                max={paramDef.max}
                step={paramDef.step || 0.1}
                onChange={(val) => handleRangeChange(paramKey, [minValue, val])}
                disabled={!enabled}
                style={{ width: 60 }}
              />
            </Tooltip>
          </Space>
        </div>
        
        {/* For brightness and contrast, show marks at -20%, 0%, +20% */}
        {showRelativeValues ? (
          <Slider
            range
            value={[minValue, maxValue]}
            min={paramDef.min}
            max={paramDef.max}
            step={paramDef.step || 0.1}
            onChange={(val) => handleRangeChange(paramKey, val)}
            disabled={!enabled}
            tooltip={{ 
              formatter: (val) => formatValue(val) 
            }}
            marks={{
              0.8: '-20%',
              1.0: 'Normal',
              1.2: '+20%'
            }}
            style={{ margin: '4px 0' }}
          />
        ) : (
          <Slider
            range
            value={[minValue, maxValue]}
            min={paramDef.min}
            max={paramDef.max}
            step={paramDef.step || 0.1}
            onChange={(val) => handleRangeChange(paramKey, val)}
            disabled={!enabled}
            tooltip={{ 
              formatter: (val) => formatValue(val) 
            }}
            style={{ margin: '4px 0' }}
          />
        )}
      </div>
    );
  };
  ```

- [x] **Add unit display for each parameter type**
  ```jsx
  // Helper function to get appropriate unit label
  const getUnitLabel = (paramKey, paramDef) => {
    const unitMap = {
      brightness: "× (multiplier)",
      contrast: "× (multiplier)",
      rotation: "° (degrees)",
      scale: "% (percent)",
      blur: "px (pixels)",
      // Add more as needed
    };
    
    return unitMap[paramKey] || paramDef.unit || "";
  };
  ```

- [x] **Set appropriate range defaults for each parameter type**
  ```jsx
  // Helper function to get default range for a parameter
  const getDefaultRange = (paramKey, currentValue) => {
    // For brightness and contrast, use +/- range around 1.0
    if (paramKey === 'brightness' || paramKey === 'contrast') {
      const defaultValue = 1.0;
      const currentVal = currentValue || defaultValue;
      // If current value is 1.0, use -0.2 to +0.2 range
      if (currentVal === defaultValue) {
        return [0.8, 1.2]; // -20% to +20% around normal
      }
      // Otherwise center the range around current value
      const rangeSize = 0.4; // Total range size
      return [
        Math.max(0.5, currentVal - rangeSize/2), 
        Math.min(2.0, currentVal + rangeSize/2)
      ];
    }
    
    // For rotation, use symmetric range around 0
    if (paramKey === 'rotation') {
      const currentVal = currentValue || 0;
      // If current value is 0, use -30 to +30 range
      if (currentVal === 0) {
        return [-30, 30];
      }
      // Otherwise center the range around current value
      const rangeSize = 60; // Total range size in degrees
      return [currentVal - rangeSize/2, currentVal + rangeSize/2];
    }
    
    // For scale, use range around 100%
    if (paramKey === 'scale') {
      const defaultValue = 100;
      const currentVal = currentValue || defaultValue;
      // If current value is 100%, use 80% to 120% range
      if (currentVal === defaultValue) {
        return [80, 120];
      }
      // Otherwise center the range around current value
      const rangeSize = 40; // Total range size in percentage points
      return [
        Math.max(50, currentVal - rangeSize/2), 
        Math.min(150, currentVal + rangeSize/2)
      ];
    }
    
    // Default behavior for other parameters
    if (currentValue !== undefined) {
      // Create a range of +/- 20% around current value
      const rangeSize = Math.abs(currentValue * 0.4);
      return [currentValue - rangeSize/2, currentValue + rangeSize/2];
    }
    
    // Fallback to parameter definition defaults
    return [paramDef.default, paramDef.default];
  };
  ```

##### 1.2 Update Parameter State Management
- [x] **Modify state structure to support ranges**
  ```jsx
  // In IndividualTransformationControl.jsx
  const [parameterRanges, setParameterRanges] = useState({});
  
  // Initialize from transformation definition and current config
  useEffect(() => {
    const initialRanges = {};
    
    if (transformation.parameters) {
      Object.entries(transformation.parameters).forEach(([key, paramDef]) => {
        // Check if range already exists in config
        if (config.ranges && config.ranges[key]) {
          initialRanges[key] = config.ranges[key];
        } else {
          // Create default range based on current single value
          const currentValue = config[key] !== undefined ? config[key] : paramDef.default;
          initialRanges[key] = [
            currentValue,
            currentValue
          ];
        }
      });
    }
    
    setParameterRanges(initialRanges);
  }, [transformation, config]);
  ```

- [x] **Update parent component with range values**
  ```jsx
  // Update parent when ranges change
  useEffect(() => {
    const newConfig = {
      enabled,
      ...parameters, // Keep single values for backward compatibility
      ranges: parameterRanges // Add ranges
    };
    onChange(newConfig);
  }, [enabled, parameters, parameterRanges, onChange]);
  ```

##### 1.3 Update renderParameterControl Function
- [x] **Modify to support both single values and ranges**
  ```jsx
  // In renderParameterControl function
  const renderParameterControl = (paramKey, paramDef) => {
    // Check if range mode is enabled for this parameter
    const isRangeMode = rangeEnabledParams.includes(paramKey);
    
    if (isRangeMode) {
      return renderRangeSlider(paramKey, paramDef);
    }
    
    // Existing single-value control code...
  };
  ```

- [x] **Add toggle to switch between single value and range**
  ```jsx
  <div style={{ 
    display: 'flex', 
    justifyContent: 'space-between', 
    alignItems: 'center',
    marginBottom: 4
  }}>
    <span style={{ fontSize: '12px', fontWeight: 500 }}>
      {paramKey.charAt(0).toUpperCase() + paramKey.slice(1)}
    </span>
    <Space>
      <span style={{ fontSize: '10px', color: '#999' }}>Range</span>
      <Switch
        size="small"
        checked={rangeEnabledParams.includes(paramKey)}
        onChange={(checked) => toggleRangeMode(paramKey, checked)}
        disabled={!enabled}
      />
    </Space>
  </div>
  ```

##### 1.4 Add Combination Count Display
- [x] **Add combination counter in TransformationModal.jsx** ✅ COMPLETED
  ```jsx
  const [combinationCount, setCombinationCount] = useState(0);
  
  // Calculate combination count whenever parameters change
  useEffect(() => {
    const calculateCombinations = () => {
      let count = 1;
      
      // For each transformation
      existingTransformations.forEach(transformation => {
        const config = transformation.config || {};
        const ranges = config.ranges || {};
        
        // For each parameter with range enabled
        Object.entries(ranges).forEach(([paramKey, range]) => {
          if (config.enabled && range[0] !== range[1]) {
            const paramDef = getParameterDefinition(transformation.type, paramKey);
            const step = paramDef?.step || 0.1;
            const possibleValues = Math.ceil((range[1] - range[0]) / step) + 1;
            count *= possibleValues;
          }
        });
      });
      
      setCombinationCount(count);
    };
    
    calculateCombinations();
  }, [existingTransformations]);
  ```

- [ ] **Display the count in the UI**
  ```jsx
  <div className="combination-counter" style={{ marginTop: 16, textAlign: 'center' }}>
    <Alert
      type={combinationCount > 100 ? "warning" : "info"}
      message={
        <span>
          <strong>{combinationCount}</strong> possible combinations
          {combinationCount > 100 && " (consider reducing ranges)"}
        </span>
      }
      showIcon
    />
  </div>
  ```

### 🔹 TASK 2: DATABASE SCHEMA UPDATE

**Priority**: 🔴 HIGH  
**Status**: ✅ COMPLETED  
**Completed Time**: 2-3 hours  
**Files to Modify**:
- `/workspace/sy-app-1/backend/database/models.py`
- Create new migration script

#### 📋 Sub-Tasks:

##### 2.1 Update Database Model
- [x] **Add parameter_ranges column to image_transformations table**
  ```python
  # In models.py
  class ImageTransformation(Base):
      __tablename__ = "image_transformations"
      
      id = Column(Integer, primary_key=True)
      name = Column(String, nullable=False)
      description = Column(String)
      parameters = Column(Text)  # Existing column for single values
      parameter_ranges = Column(Text)  # New column for ranges
      # Other existing columns...
  ```

- [x] **Create migration script**
  ```python
  # Create add_parameter_ranges_migration.py
  from alembic import op
  import sqlalchemy as sa
  
  def upgrade():
      op.add_column('image_transformations', sa.Column('parameter_ranges', sa.Text))
      
  def downgrade():
      op.drop_column('image_transformations', 'parameter_ranges')
  ```

##### 2.2 Update API Endpoints
- [x] **Modify transformation save endpoint**
  ```python
  # In image_transformations.py
  @router.post("/image-transformations")
  async def save_transformation(transformation_data: dict):
      # Extract single values and ranges
      parameters = transformation_data.get("parameters", {})
      parameter_ranges = transformation_data.get("parameter_ranges", {})
      
      # Convert to JSON strings
      parameters_json = json.dumps(parameters)
      parameter_ranges_json = json.dumps(parameter_ranges)
      
      # Save to database
      transformation = ImageTransformation(
          name=transformation_data.get("name"),
          description=transformation_data.get("description"),
          parameters=parameters_json,
          parameter_ranges=parameter_ranges_json,
          # Other fields...
      )
      
      db.add(transformation)
      db.commit()
      # Rest of function...
  ```

- [ ] **Update transformation retrieval endpoint**
  ```python
  # In image_transformations.py
  @router.get("/image-transformations/{transformation_id}")
  async def get_transformation(transformation_id: int):
      transformation = db.query(ImageTransformation).filter(
          ImageTransformation.id == transformation_id
      ).first()
      
      if not transformation:
          raise HTTPException(status_code=404, detail="Transformation not found")
      
      # Parse JSON strings
      parameters = json.loads(transformation.parameters) if transformation.parameters else {}
      parameter_ranges = json.loads(transformation.parameter_ranges) if transformation.parameter_ranges else {}
      
      return {
          "id": transformation.id,
          "name": transformation.name,
          "description": transformation.description,
          "parameters": parameters,
          "parameter_ranges": parameter_ranges,
          # Other fields...
      }
  ```

### 🔹 TASK 3: TRANSFORMATION COMBINATION GENERATOR

**Priority**: 🔴 HIGH  
**Status**: ❌ NOT STARTED  
**Estimated Time**: 3-4 hours  
**Files to Create/Modify**:
- Create `/workspace/sy-app-1/backend/schema.py`
- Modify `/workspace/sy-app-1/backend/api/routes/releases.py`

#### 📋 Sub-Tasks:

##### 3.1 Create Schema.py with Range-to-Values Converter
- [ ] **Implement function to generate values from range**
  ```python
  # In schema.py
  def generate_values_from_range(min_val, max_val, step, count=None):
      """
      Generate a list of values from a range.
      
      Args:
          min_val: Minimum value
          max_val: Maximum value
          step: Step size
          count: Number of values to generate (if None, use step)
          
      Returns:
          List of values within the range
      """
      if min_val == max_val:
          return [min_val]  # Single value case
          
      if count is None:
          # Generate all possible values based on step
          values = []
          current = min_val
          while current <= max_val:
              values.append(round(current, 6))  # Round to avoid floating point issues
              current += step
          return values
      else:
          # Generate exactly count values evenly distributed
          if count == 1:
              return [(min_val + max_val) / 2]  # Middle value
          
          step_size = (max_val - min_val) / (count - 1)
          return [round(min_val + i * step_size, 6) for i in range(count)]
  ```

##### 3.2 Implement Combination Generator
- [ ] **Create function to generate combinations**
  ```python
  # In schema.py
  import itertools
  
  def generate_combinations(parameter_ranges, tool_definitions, max_combinations=None):
      """
      Generate combinations of parameter values from ranges.
      
      Args:
          parameter_ranges: Dict of parameter ranges
          tool_definitions: Dict of tool definitions with step sizes
          max_combinations: Maximum number of combinations to generate
          
      Returns:
          List of parameter combinations
      """
      # For each enabled parameter, generate value list
      parameter_values = {}
      for tool_name, tool_config in parameter_ranges.items():
          if tool_config.get("enabled", False):
              for param_name, range_config in tool_config.get("ranges", {}).items():
                  if range_config[0] != range_config[1]:  # Only if it's a real range
                      step = get_step_for_parameter(tool_definitions, tool_name, param_name)
                      values = generate_values_from_range(
                          range_config[0],
                          range_config[1],
                          step
                      )
                      parameter_values[f"{tool_name}.{param_name}"] = values
                  else:
                      # Use single value if min == max
                      parameter_values[f"{tool_name}.{param_name}"] = [range_config[0]]
          else:
              # Skip disabled tools
              continue
      
      # Generate all combinations using itertools.product
      all_names = list(parameter_values.keys())
      all_values = [parameter_values[name] for name in all_names]
      
      all_combinations = list(itertools.product(*all_values))
      
      # Convert to list of dicts
      result = []
      for combo in all_combinations:
          combo_dict = {}
          for i, name in enumerate(all_names):
              tool_name, param_name = name.split(".")
              if tool_name not in combo_dict:
                  combo_dict[tool_name] = {}
              combo_dict[tool_name][param_name] = combo[i]
          result.append(combo_dict)
      
      # Limit to max_combinations if specified
      if max_combinations and len(result) > max_combinations:
          # Implement smart sampling strategy
          return smart_sample_combinations(result, max_combinations)
      
      return result
  ```

##### 3.3 Implement Combination Count Calculator
- [ ] **Create function to calculate total possible combinations**
  ```python
  # In schema.py
  def calculate_total_combinations(parameter_ranges, tool_definitions):
      """
      Calculate the total number of possible combinations.
      
      Args:
          parameter_ranges: Dict of parameter ranges
          tool_definitions: Dict of tool definitions with step sizes
          
      Returns:
          Total number of possible combinations
      """
      total = 1
      for tool_name, tool_config in parameter_ranges.items():
          if tool_config.get("enabled", False):
              for param_name, range_config in tool_config.get("ranges", {}).items():
                  if range_config[0] != range_config[1]:  # Only if it's a real range
                      min_val = range_config[0]
                      max_val = range_config[1]
                      step = get_step_for_parameter(tool_definitions, tool_name, param_name)
                      
                      # Calculate number of possible values for this parameter
                      num_values = max(1, int(round((max_val - min_val) / step)) + 1)
                      
                      # Multiply by total
                      total *= num_values
      
      return total
  ```

##### 3.4 Implement Smart Sampling Strategy
- [ ] **Create function for intelligent sampling**
  ```python
  # In schema.py
  def smart_sample_combinations(combinations, max_samples):
      """
      Intelligently sample combinations to ensure good coverage.
      
      Args:
          combinations: List of all combinations
          max_samples: Maximum number of samples to return
          
      Returns:
          Sampled combinations
      """
      if len(combinations) <= max_samples:
          return combinations
          
      # Always include first and last combination (min and max values)
      result = [combinations[0], combinations[-1]]
      
      # Include middle combination
      middle_idx = len(combinations) // 2
      result.append(combinations[middle_idx])
      
      # Randomly sample the rest
      import random
      remaining = max_samples - len(result)
      
      # Remove already selected combinations
      available = [c for i, c in enumerate(combinations) 
                  if i != 0 and i != len(combinations) - 1 and i != middle_idx]
      
      # Random sample without replacement
      if remaining > 0 and available:
          result.extend(random.sample(available, min(remaining, len(available))))
      
      return result
  ```

##### 3.5 Add API Endpoint for Combination Count
- [ ] **Create endpoint to calculate combination count**
  ```python
  # In releases.py
  from ..schema import calculate_total_combinations
  
  @router.get("/releases/versions/{version_id}/combinations")
  async def get_combination_count(version_id: int):
      # Load transformation ranges for this version
      transformation = await load_transformation(version_id)
      
      # Get tool definitions
      tool_definitions = await load_tool_definitions()
      
      # Parse parameter ranges
      parameter_ranges = json.loads(transformation.parameter_ranges) if transformation.parameter_ranges else {}
      
      # Calculate combination count
      count = calculate_total_combinations(parameter_ranges, tool_definitions)
      
      # Calculate recommended max (limit to reasonable number)
      max_recommended = min(count, 10)
      
      return {
          "count": count, 
          "max_recommended": max_recommended
      }
  ```

### 🔹 TASK 4: IMAGE AUGMENTATION ENGINE

**Priority**: 🔴 HIGH  
**Status**: ❌ NOT STARTED  
**Estimated Time**: 4-5 hours  
**Files to Create/Modify**:
- Create `/workspace/sy-app-1/backend/image_generator.py`
- Modify `/workspace/sy-app-1/backend/api/routes/releases.py`

#### 📋 Sub-Tasks:

##### 4.1 Implement Core Transformation Functions
- [ ] **Create base transformation functions**
  ```python
  # In image_generator.py
  import cv2
  import numpy as np
  
  def apply_brightness(image, factor):
      """Apply brightness transformation to image"""
      hsv = cv2.cvtColor(image, cv2.COLOR_BGR2HSV)
      hsv = hsv.astype(np.float32)
      hsv[:, :, 2] = hsv[:, :, 2] * factor
      hsv[:, :, 2] = np.clip(hsv[:, :, 2], 0, 255)
      hsv = hsv.astype(np.uint8)
      return cv2.cvtColor(hsv, cv2.COLOR_HSV2BGR)
  
  def apply_contrast(image, factor):
      """Apply contrast transformation to image"""
      mean = np.mean(image, axis=(0, 1))
      return np.clip((image - mean) * factor + mean, 0, 255).astype(np.uint8)
  
  def apply_rotation(image, angle):
      """Apply rotation transformation to image"""
      h, w = image.shape[:2]
      center = (w // 2, h // 2)
      M = cv2.getRotationMatrix2D(center, angle, 1.0)
      return cv2.warpAffine(image, M, (w, h), borderMode=cv2.BORDER_REFLECT)
  
  # Add more transformation functions as needed...
  ```

##### 4.2 Implement Annotation Update Functions
- [ ] **Create functions to update annotations**
  ```python
  # In image_generator.py
  def transform_bbox(bbox, transformation_matrix, image_width, image_height):
      """
      Transform bounding box coordinates using transformation matrix.
      
      Args:
          bbox: [x, y, width, height] in absolute coordinates
          transformation_matrix: 2x3 transformation matrix
          image_width: Width of the image
          image_height: Height of the image
          
      Returns:
          Transformed bbox [x, y, width, height]
      """
      # Convert bbox to corner points
      x, y, w, h = bbox
      points = np.array([
          [x, y],
          [x + w, y],
          [x + w, y + h],
          [x, y + h]
      ], dtype=np.float32)
      
      # Apply transformation
      ones = np.ones((points.shape[0], 1), dtype=np.float32)
      points_homogeneous = np.hstack([points, ones])
      transformed_points = np.dot(points_homogeneous, transformation_matrix.T)
      
      # Calculate new bbox
      min_x = max(0, np.min(transformed_points[:, 0]))
      min_y = max(0, np.min(transformed_points[:, 1]))
      max_x = min(image_width, np.max(transformed_points[:, 0]))
      max_y = min(image_height, np.max(transformed_points[:, 1]))
      
      return [min_x, min_y, max_x - min_x, max_y - min_y]
  
  def transform_polygon(polygon_points, transformation_matrix):
      """
      Transform polygon coordinates using transformation matrix.
      
      Args:
          polygon_points: List of [x, y] points
          transformation_matrix: 2x3 transformation matrix
          
      Returns:
          Transformed polygon points
      """
      points = np.array(polygon_points, dtype=np.float32)
      
      # Apply transformation
      ones = np.ones((points.shape[0], 1), dtype=np.float32)
      points_homogeneous = np.hstack([points, ones])
      transformed_points = np.dot(points_homogeneous, transformation_matrix.T)
      
      return transformed_points.tolist()
  ```

##### 4.3 Implement Main Augmentation Function
- [ ] **Create function to generate augmented images**
  ```python
  # In image_generator.py
  def generate_augmented_image(
      original_image_path,
      transformation_config,
      original_annotations,
      output_path,
      original_dataset_split
  ):
      """
      Generate augmented image with updated annotations.
      
      Args:
          original_image_path: Path to original image
          transformation_config: Dict of transformation parameters
          original_annotations: List of annotation objects
          output_path: Path to save augmented image
          original_dataset_split: Original split (train/val/test)
          
      Returns:
          Dict with paths to augmented image and updated annotations
      """
      # Load original image
      image = cv2.imread(original_image_path)
      if image is None:
          raise ValueError(f"Failed to load image: {original_image_path}")
      
      # Initialize transformation matrix as identity
      h, w = image.shape[:2]
      transformation_matrix = np.eye(3, dtype=np.float32)
      
      # Create directory for augmented image based on original split
      # CRITICAL: Save to /projects/gevis/augmented/{split}/ NOT back to original dataset
      augmented_dir = f"/projects/gevis/augmented/{original_dataset_split}"
      os.makedirs(augmented_dir, exist_ok=True)
      
      # Get original filename and create augmented filename
      original_filename = os.path.basename(original_image_path)
      base_name, ext = os.path.splitext(original_filename)
      augmented_filename = f"{base_name}_aug{transformation_config.get('config_id', 1)}{ext}"
      
      # Set final output path in the augmented directory
      final_output_path = os.path.join(augmented_dir, augmented_filename)
      
      # Apply transformations and update matrix
      for transform_type, params in transformation_config.items():
          if transform_type == "brightness" and params.get("enabled", False):
              factor = params.get("factor", 1.0)
              image = apply_brightness(image, factor)
          
          elif transform_type == "contrast" and params.get("enabled", False):
              factor = params.get("factor", 1.0)
              image = apply_contrast(image, factor)
          
          elif transform_type == "rotation" and params.get("enabled", False):
              angle = params.get("angle", 0)
              # Update transformation matrix for rotation
              center = (w // 2, h // 2)
              M = cv2.getRotationMatrix2D(center, angle, 1.0)
              # Convert to 3x3 matrix
              M_3x3 = np.eye(3, dtype=np.float32)
              M_3x3[:2, :] = M
              transformation_matrix = np.dot(M_3x3, transformation_matrix)
              # Apply rotation
              image = apply_rotation(image, angle)
          
          # Add more transformations as needed...
      
      # Save augmented image to the correct augmented directory
      cv2.imwrite(final_output_path, image)
      
      # Update annotations
      updated_annotations = []
      for ann in original_annotations:
          updated_ann = ann.copy()
          
          # Update bounding box
          if "bbox" in ann:
              updated_ann["bbox"] = transform_bbox(
                  ann["bbox"], 
                  transformation_matrix[:2, :], 
                  w, h
              )
          
          # Update polygon points
          if "segmentation" in ann and ann["segmentation"]:
              updated_ann["segmentation"] = [
                  transform_polygon(points, transformation_matrix[:2, :])
                  for points in ann["segmentation"]
              ]
          
          # Add reference to the augmented image
          updated_ann["image_path"] = final_output_path
          updated_ann["original_image_path"] = original_image_path
          updated_ann["dataset_split"] = original_dataset_split
          
          updated_annotations.append(updated_ann)
      
      # Log the augmentation
      print(f"Generated augmented image: {final_output_path}")
      print(f"Applied transformations: {list(transformation_config.keys())}")
      print(f"Original split: {original_dataset_split}")
      
      return {
          "augmented_image_path": final_output_path,
          "updated_annotations": updated_annotations,
          "original_image_path": original_image_path,
          "dataset_split": original_dataset_split,
          "transformation_config": transformation_config
      }
  ```

##### 4.4 Implement Batch Processing
- [ ] **Create function for batch augmentation**
  ```python
  # In image_generator.py
  import os
  import json
  from concurrent.futures import ThreadPoolExecutor
  
  def batch_generate_augmentations(
      dataset_images,
      transformation_combinations,
      output_dir,
      annotations_by_image_id,
      max_workers=4
  ):
      """
      Generate augmented images for a batch of images.
      
      Args:
          dataset_images: List of image objects with paths
          transformation_combinations: List of transformation parameter combinations
          output_dir: Directory to save augmented images
          annotations_by_image_id: Dict mapping image IDs to annotations
          max_workers: Maximum number of parallel workers
          
      Returns:
          Dict mapping original image IDs to lists of augmented images and annotations
      """
      os.makedirs(output_dir, exist_ok=True)
      
      results = {}
      
      def process_image(image):
          image_id = image["id"]
          image_path = image["path"]
          image_filename = os.path.basename(image_path)
          base_name = os.path.splitext(image_filename)[0]
          
          # Get annotations for this image
          annotations = annotations_by_image_id.get(image_id, [])
          
          image_results = []
          
          for i, combo in enumerate(transformation_combinations):
              # Create output path
              output_filename = f"{base_name}_aug{i+1}{os.path.splitext(image_filename)[1]}"
              output_path = os.path.join(output_dir, output_filename)
              
              try:
                  # Generate augmented image
                  result = generate_augmented_image(
                      image_path,
                      combo,
                      annotations,
                      output_path
                  )
                  
                  image_results.append({
                      "augmented_image_path": result["augmented_image_path"],
                      "updated_annotations": result["updated_annotations"],
                      "transformation_config": combo
                  })
              except Exception as e:
                  print(f"Error processing {image_path} with combo {i}: {str(e)}")
          
          return image_id, image_results
      
      # Process images in parallel
      with ThreadPoolExecutor(max_workers=max_workers) as executor:
          for image_id, image_results in executor.map(process_image, dataset_images):
              results[image_id] = image_results
      
      return results
  ```

### 🔹 TASK 5: CENTRAL RELEASE CONTROLLER

**Priority**: 🔴 HIGH  
**Status**: ❌ NOT STARTED  
**Estimated Time**: 3-4 hours  
**Files to Create/Modify**:
- Create `/workspace/sy-app-1/backend/release.py`
- Modify `/workspace/sy-app-1/backend/api/routes/releases.py`

#### 📋 Sub-Tasks:

##### 5.1 Implement Release Controller
- [ ] **Create main release orchestration function**
  ```python
  # In release.py
  from .schema import generate_combinations, calculate_total_combinations
  from .image_generator import batch_generate_augmentations
  
  async def orchestrate_release(release_config):
      """
      Orchestrate the entire release generation process.
      
      Args:
          release_config: Dict with release configuration
          
      Returns:
          Dict with release results
      """
      # Extract configuration
      release_version = release_config.get("name")
      datasets = release_config.get("selectedDatasets", [])
      transformations = release_config.get("transformations", [])
      images_per_original = release_config.get("multiplier", 5)
      
      # Load transformation configurations
      transformation_configs = []
      for t in transformations:
          # Load from database
          transformation = await load_transformation(t.get("id"))
          transformation_configs.append(transformation)
      
      # Parse parameter ranges
      parameter_ranges = {}
      for t in transformation_configs:
          if t.parameter_ranges:
              ranges = json.loads(t.parameter_ranges)
              parameter_ranges[t.name] = ranges
      
      # Get tool definitions
      tool_definitions = await load_tool_definitions()
      
      # Generate combinations
      combinations = generate_combinations(
          parameter_ranges,
          tool_definitions,
          max_combinations=images_per_original
      )
      
      # Load dataset images and annotations
      dataset_images = []
      annotations_by_image_id = {}
      
      for dataset_id in datasets:
          # Load images from dataset
          images = await load_dataset_images(dataset_id)
          dataset_images.extend(images)
          
          # Load annotations for each image
          for image in images:
              annotations = await load_image_annotations(image["id"])
              annotations_by_image_id[image["id"]] = annotations
      
      # Create output directory
      output_dir = f"/tmp/releases/{release_version}"
      os.makedirs(output_dir, exist_ok=True)
      
      # Generate augmented images
      augmentation_results = batch_generate_augmentations(
          dataset_images,
          combinations,
          output_dir,
          annotations_by_image_id
      )
      
      # Prepare for export
      export_config = {
          "format": release_config.get("exportFormat", "yolo_detection"),
          "task_type": release_config.get("taskType", "object_detection"),
          "release_version": release_version,
          "augmentation_results": augmentation_results
      }
      
      # Trigger export
      export_result = await trigger_export(export_config)
      
      return {
          "release_version": release_version,
          "total_original_images": len(dataset_images),
          "total_augmented_images": len(dataset_images) * len(combinations),
          "export_path": export_result["export_path"]
      }
  ```

##### 5.2 Implement API Endpoints
- [ ] **Create endpoint to generate release**
  ```python
  # In releases.py
  from ..release import orchestrate_release
  
  @router.post("/releases/generate")
  async def generate_release(release_config: dict):
      try:
          # Validate release configuration
          if not release_config.get("name"):
              raise HTTPException(status_code=400, detail="Release name is required")
          
          if not release_config.get("selectedDatasets"):
              raise HTTPException(status_code=400, detail="Selected datasets are required")
          
          if not release_config.get("transformations"):
              raise HTTPException(status_code=400, detail="Transformations are required")
          
          # Orchestrate release generation
          result = await orchestrate_release(release_config)
          
          return {
              "success": True,
              "message": "Release generated successfully",
              "data": result
          }
      except Exception as e:
          return {
              "success": False,
              "message": f"Failed to generate release: {str(e)}"
          }
  ```

- [ ] **Create endpoint to get release status**
  ```python
  # In releases.py
  @router.get("/releases/{release_version}/status")
  async def get_release_status(release_version: str):
      try:
          # Query database for release status
          release = await get_release_by_version(release_version)
          
          if not release:
              raise HTTPException(status_code=404, detail="Release not found")
          
          return {
              "success": True,
              "data": {
                  "release_version": release_version,
                  "status": release.status,
                  "progress": release.progress,
                  "created_at": release.created_at,
                  "completed_at": release.completed_at
              }
          }
      except Exception as e:
          return {
              "success": False,
              "message": f"Failed to get release status: {str(e)}"
          }
  ```

## 🔄 IMPLEMENTATION STRATEGY

### Phase 1: Two-Point Slider UI (2-3 days)
1. Implement RangeSlider component in IndividualTransformationControl.jsx
2. Update state management to support ranges
3. Add parameter enable/disable toggle
4. Add combination count display

### Phase 2: Backend Support (2-3 days)
1. Update database schema for parameter ranges
2. Implement schema.py with combination generator
3. Create image_generator.py for augmentation
4. Implement release.py for orchestration

### Phase 3: Integration & Testing (1-2 days)
1. Connect frontend and backend
2. Test with various parameter types
3. Verify combination calculation
4. Test end-to-end release generation

## 🧪 TESTING REQUIREMENTS

### Frontend Testing
- **Unit Tests**:
  - Test RangeSlider component with different parameter types
  - Verify range validation (min < max)
  - Test combination count calculation
  - Test parameter enable/disable functionality

- **Integration Tests**:
  - Test TransformationModal with RangeSlider components
  - Verify state management for ranges
  - Test API integration for saving ranges

### Backend Testing
- **Unit Tests**:
  - Test range-to-values conversion
  - Test combination generation
  - Test smart sampling strategy
  - Test annotation transformation functions

- **Integration Tests**:
  - Test end-to-end release generation
  - Verify augmented images match transformation parameters
  - Test export formats with augmented images
  - Verify annotation updates are correct

### Performance Testing
- Test with large datasets (100+ images)
- Measure memory usage during combination generation
- Test with complex transformation combinations
- Verify UI responsiveness during processing

## 📅 NEXT STEPS

1. **Immediate Actions**
   - Start with RangeSlider component implementation
   - Update database schema for range storage
   - Implement basic combination calculator

2. **Testing Plan**
   - Unit tests for range slider component
   - Integration tests for combination generator
   - End-to-end tests for release generation

3. **Documentation**
   - Update API documentation
   - Create user guide for range-based transformations
   - Document release generation workflow

## 🚨 CRITICAL IMPLEMENTATION CONSIDERATIONS

### Parameter Range Handling
- **Brightness and Contrast**: Display as +/- percentage from normal (1.0)
  - Example: -20% to +20% (0.8 to 1.2)
  - Normal value is 1.0 (no change)
  - UI should show relative percentages for intuitive understanding

- **Rotation**: Display in degrees with +/- values
  - Example: -30° to +30°
  - Zero is no rotation
  - Support for full 360° rotation if needed

- **Scale**: Display as percentage of original size
  - Example: 80% to 120%
  - 100% is original size
  - Maintain aspect ratio during scaling

### Combination Calculation Logic
- Total combinations = product of all possible values across enabled parameters
- For each parameter with range [min, max, step]:
  - Number of values = (max - min) / step + 1
- Example:
  - Brightness: [0.8, 1.2] with step 0.1 = 5 values
  - Rotation: [-30, 30] with step 15 = 5 values
  - Total combinations = 5 × 5 = 25 possible combinations

### Smart Sampling Strategy
- When total combinations exceed "Images per Original" limit:
  1. Always include min/max extremes for each parameter
  2. Include center/normal values
  3. Sample remaining combinations intelligently
  4. Ensure good coverage of parameter space

### File System Organization and Data Flow

#### Important Path Structure
```
projects/
└── gevis/
    ├── dataset/                  # Original datasets organized by dataset name
    │   ├── animal/               # Example dataset
    │   │   ├── train/            # Original training images
    │   │   ├── val/              # Original validation images
    │   │   └── test/             # Original test images
    │   └── other_dataset/        # Another dataset with its own splits
    ├── augmented/                # CRITICAL: All augmented images go here, NOT in dataset folders
    │   ├── train/                # All augmented training images from ALL datasets
    │   ├── val/                  # All augmented validation images from ALL datasets
    │   └── test/                 # All augmented test images from ALL datasets
    └── release/                  # Final packaged releases as ZIP files
        └── v1_brightness_yolo.zip
```

#### Data Flow Process
1. **Original Images**: Located in `/projects/gevis/dataset/{dataset_name}/{split}/`
   - Each dataset has its own train/val/test folders
   - Original annotations are stored with these images
   - **IMPORTANT**: Original images remain untouched in their original folders
   - Original dataset structure is preserved throughout the process

2. **Augmentation Process**:
   - System reads images from original dataset folders
   - Applies transformations based on parameter ranges
   - Saves augmented images to `/projects/gevis/augmented/{split}/`
   - **CRITICAL**: All augmented images go to the common augmented folder, NOT back to original dataset folders
   - Original images are never modified or moved
   - Naming convention: `{original_filename}_aug{index}.{extension}`

3. **Annotation Updates**:
   - When transformations are applied, annotations must be updated
   - For rotations: bounding box coordinates are transformed
   - For flips: coordinates are mirrored
   - For scaling: coordinates are scaled proportionally
   - Updated annotations are stored with references to augmented images

4. **Release Generation**:
   - System collects all augmented images from `/projects/gevis/augmented/{split}/`
   - Organizes them according to export format requirements
   - Creates a ZIP package in `/projects/gevis/release/`
   - Naming convention: `{release_version}_{transformation_types}_{format}.zip`
   - **IMPORTANT**: Original dataset remains intact and available in its original location
   - The release process is non-destructive and creates new files without modifying originals

#### Implementation Details
- Backend must track which original images produced which augmented images
- Augmented image paths must be stored in the database with references to original images
- When generating releases, the system must gather all relevant augmented images across all splits
- The export system must organize the files according to the chosen format's requirements
- **CRITICAL**: The system maintains a clear separation between:
  1. Original dataset images (remain untouched in `/projects/gevis/dataset/`)
  2. Augmented images (stored in `/projects/gevis/augmented/`)
  3. Release packages (stored in `/projects/gevis/release/`)
- This separation ensures data integrity and allows users to generate multiple releases from the same original dataset