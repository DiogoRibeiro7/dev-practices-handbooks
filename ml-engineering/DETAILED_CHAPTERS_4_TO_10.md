# Detailed Chapter Outlines: Chapters 4-10

## Chapter 4: Model Development Lifecycle

### 4.1 Model Selection Strategy

**Content:**
- Problem-model matching framework
- Algorithm comparison methodology
- Baseline model establishment
- Model complexity vs performance trade-offs
- Decision trees for model selection

**Practical Example:**
- Multi-model comparison study
- Baseline vs complex model analysis

**Code Listing 4.1: Automated Model Selection**
```python
from sklearn.ensemble import RandomForestClassifier, GradientBoostingClassifier
from sklearn.linear_model import LogisticRegression
from sklearn.svm import SVC
from sklearn.neural_network import MLPClassifier
from xgboost import XGBClassifier
from sklearn.model_selection import cross_val_score
from typing import Dict, List, Tuple
import pandas as pd
import time

class ModelSelector:
    """Automated model selection framework"""

    def __init__(self, X, y, cv=5, scoring='accuracy'):
        self.X = X
        self.y = y
        self.cv = cv
        self.scoring = scoring
        self.results = []

    def get_candidate_models(self) -> Dict:
        """Define candidate models"""
        return {
            'logistic_regression': LogisticRegression(max_iter=1000),
            'random_forest': RandomForestClassifier(n_estimators=100, random_state=42),
            'gradient_boosting': GradientBoostingClassifier(n_estimators=100, random_state=42),
            'xgboost': XGBClassifier(n_estimators=100, random_state=42),
            'svm': SVC(kernel='rbf', probability=True, random_state=42),
            'mlp': MLPClassifier(hidden_layers=(100, 50), max_iter=500, random_state=42)
        }

    def evaluate_model(
        self,
        name: str,
        model,
        verbose: bool = True
    ) -> Dict:
        """Evaluate single model"""

        # Training time
        start_time = time.time()
        scores = cross_val_score(
            model, self.X, self.y,
            cv=self.cv,
            scoring=self.scoring,
            n_jobs=-1
        )
        training_time = time.time() - start_time

        # Inference time (approximate)
        model.fit(self.X[:1000], self.y[:1000])
        start_time = time.time()
        _ = model.predict(self.X[:100])
        inference_time = (time.time() - start_time) / 100  # Per sample

        result = {
            'model_name': name,
            'mean_score': scores.mean(),
            'std_score': scores.std(),
            'min_score': scores.min(),
            'max_score': scores.max(),
            'training_time': training_time,
            'inference_time_ms': inference_time * 1000,
            'model_complexity': self._estimate_complexity(model)
        }

        if verbose:
            print(f"\n{name}:")
            print(f"  Score: {result['mean_score']:.4f} (+/- {result['std_score']:.4f})")
            print(f"  Training time: {result['training_time']:.2f}s")
            print(f"  Inference: {result['inference_time_ms']:.2f}ms per sample")

        return result

    def evaluate_all_models(self) -> pd.DataFrame:
        """Evaluate all candidate models"""
        models = self.get_candidate_models()

        for name, model in models.items():
            try:
                result = self.evaluate_model(name, model)
                self.results.append(result)
            except Exception as e:
                print(f"Error evaluating {name}: {e}")

        # Create results dataframe
        df_results = pd.DataFrame(self.results)
        df_results = df_results.sort_values('mean_score', ascending=False)

        return df_results

    def select_best_model(
        self,
        constraints: Dict = None
    ) -> Tuple[str, float]:
        """Select best model based on constraints"""

        if not self.results:
            raise ValueError("No models evaluated yet")

        df = pd.DataFrame(self.results)

        # Apply constraints
        if constraints:
            if 'max_inference_time_ms' in constraints:
                df = df[df['inference_time_ms'] <= constraints['max_inference_time_ms']]

            if 'max_training_time' in constraints:
                df = df[df['training_time'] <= constraints['max_training_time']]

            if 'max_complexity' in constraints:
                df = df[df['model_complexity'] <= constraints['max_complexity']]

        if df.empty:
            raise ValueError("No models meet the constraints")

        # Select model with best score
        best_model = df.iloc[0]

        return best_model['model_name'], best_model['mean_score']

    @staticmethod
    def _estimate_complexity(model) -> str:
        """Estimate model complexity"""
        if isinstance(model, LogisticRegression):
            return 'low'
        elif isinstance(model, (RandomForestClassifier, GradientBoostingClassifier)):
            return 'medium'
        elif isinstance(model, (XGBClassifier, MLPClassifier)):
            return 'high'
        elif isinstance(model, SVC):
            return 'medium'
        return 'unknown'

# Usage
if __name__ == "__main__":
    from sklearn.datasets import make_classification
    from sklearn.model_selection import train_test_split

    # Generate sample data
    X, y = make_classification(
        n_samples=10000,
        n_features=20,
        n_informative=15,
        n_redundant=5,
        random_state=42
    )

    X_train, X_test, y_train, y_test = train_test_split(
        X, y, test_size=0.2, random_state=42
    )

    # Select model
    selector = ModelSelector(X_train, y_train, cv=5)
    results_df = selector.evaluate_all_models()

    print("\n=== Model Comparison ===")
    print(results_df)

    # Select with constraints
    constraints = {
        'max_inference_time_ms': 1.0,  # Max 1ms per prediction
        'max_training_time': 30.0       # Max 30s training
    }

    best_model, best_score = selector.select_best_model(constraints)
    print(f"\nBest model: {best_model} with score {best_score:.4f}")
```

### 4.2 Iterative Model Development

**Content:**
- Baseline → Simple → Complex model progression
- Incremental improvement strategies
- Model debugging techniques
- Learning curves analysis
- Bias-variance trade-off

**Practical Example:**
- Progressive model improvement case study
- Debugging underperforming models

**Code Listing 4.2: Model Development Iteration**
```python
import numpy as np
import pandas as pd
from sklearn.model_selection import learning_curve, validation_curve
import matplotlib.pyplot as plt
from typing import Dict, List

class ModelDevelopmentFramework:
    """Framework for iterative model development"""

    def __init__(self, X, y):
        self.X = X
        self.y = y
        self.history = []

    def baseline_model(self) -> Dict:
        """Establish baseline model"""
        from sklearn.dummy import DummyClassifier

        baseline = DummyClassifier(strategy='most_frequent')
        baseline.fit(self.X, self.y)
        score = baseline.score(self.X, self.y)

        result = {
            'iteration': 0,
            'model_type': 'baseline',
            'description': 'Most frequent class',
            'score': score,
            'notes': 'Random baseline for comparison'
        }

        self.history.append(result)
        return result

    def simple_model(self) -> Dict:
        """Try simple linear model"""
        from sklearn.linear_model import LogisticRegression
        from sklearn.model_selection import cross_val_score

        model = LogisticRegression(max_iter=1000)
        scores = cross_val_score(model, self.X, self.y, cv=5)

        result = {
            'iteration': 1,
            'model_type': 'logistic_regression',
            'description': 'Simple linear model',
            'score': scores.mean(),
            'std': scores.std(),
            'notes': 'Linear baseline'
        }

        self.history.append(result)
        return result

    def tree_based_model(self, hyperparameters: Dict = None) -> Dict:
        """Try tree-based model"""
        from sklearn.ensemble import RandomForestClassifier
        from sklearn.model_selection import cross_val_score

        params = hyperparameters or {'n_estimators': 100, 'max_depth': 10}
        model = RandomForestClassifier(**params, random_state=42)
        scores = cross_val_score(model, self.X, self.y, cv=5)

        result = {
            'iteration': len(self.history),
            'model_type': 'random_forest',
            'description': f'Tree-based with {params}',
            'score': scores.mean(),
            'std': scores.std(),
            'hyperparameters': params,
            'notes': 'Non-linear model'
        }

        self.history.append(result)
        return result

    def analyze_learning_curve(self, model):
        """Analyze learning curve to diagnose bias/variance"""

        train_sizes, train_scores, val_scores = learning_curve(
            model, self.X, self.y,
            cv=5,
            n_jobs=-1,
            train_sizes=np.linspace(0.1, 1.0, 10),
            scoring='accuracy'
        )

        # Calculate means and stds
        train_mean = train_scores.mean(axis=1)
        train_std = train_scores.std(axis=1)
        val_mean = val_scores.mean(axis=1)
        val_std = val_scores.std(axis=1)

        # Plot
        plt.figure(figsize=(10, 6))
        plt.plot(train_sizes, train_mean, label='Training score')
        plt.plot(train_sizes, val_mean, label='Cross-validation score')
        plt.fill_between(train_sizes, train_mean - train_std,
                         train_mean + train_std, alpha=0.1)
        plt.fill_between(train_sizes, val_mean - val_std,
                         val_mean + val_std, alpha=0.1)
        plt.xlabel('Training Set Size')
        plt.ylabel('Accuracy Score')
        plt.title('Learning Curve')
        plt.legend(loc='best')
        plt.grid(True)

        # Diagnosis
        gap = train_mean[-1] - val_mean[-1]
        if gap > 0.1:
            diagnosis = "High variance (overfitting) - try regularization or more data"
        elif val_mean[-1] < 0.7:
            diagnosis = "High bias (underfitting) - try more complex model"
        else:
            diagnosis = "Good fit"

        plt.text(
            train_sizes[len(train_sizes)//2],
            0.5,
            diagnosis,
            bbox=dict(boxstyle='round', facecolor='wheat', alpha=0.5)
        )

        return {
            'train_score': train_mean[-1],
            'val_score': val_mean[-1],
            'gap': gap,
            'diagnosis': diagnosis
        }

    def analyze_validation_curve(self, model, param_name, param_range):
        """Analyze validation curve for hyperparameter"""

        train_scores, val_scores = validation_curve(
            model, self.X, self.y,
            param_name=param_name,
            param_range=param_range,
            cv=5,
            scoring='accuracy',
            n_jobs=-1
        )

        train_mean = train_scores.mean(axis=1)
        train_std = train_scores.std(axis=1)
        val_mean = val_scores.mean(axis=1)
        val_std = val_scores.std(axis=1)

        # Plot
        plt.figure(figsize=(10, 6))
        plt.plot(param_range, train_mean, label='Training score')
        plt.plot(param_range, val_mean, label='Cross-validation score')
        plt.fill_between(param_range, train_mean - train_std,
                         train_mean + train_std, alpha=0.1)
        plt.fill_between(param_range, val_mean - val_std,
                         val_mean + val_std, alpha=0.1)
        plt.xlabel(param_name)
        plt.ylabel('Accuracy Score')
        plt.title(f'Validation Curve - {param_name}')
        plt.legend(loc='best')
        plt.grid(True)

        # Find optimal parameter
        optimal_idx = val_mean.argmax()
        optimal_param = param_range[optimal_idx]

        return {
            'optimal_value': optimal_param,
            'optimal_score': val_mean[optimal_idx]
        }

    def get_improvement_summary(self) -> pd.DataFrame:
        """Summarize improvements across iterations"""
        df = pd.DataFrame(self.history)
        df['improvement'] = df['score'].diff()
        df['cumulative_improvement'] = df['score'] - df['score'].iloc[0]

        return df

# Usage
if __name__ == "__main__":
    from sklearn.datasets import make_classification

    X, y = make_classification(n_samples=5000, n_features=20, random_state=42)

    framework = ModelDevelopmentFramework(X, y)

    # Iteration 0: Baseline
    baseline_result = framework.baseline_model()
    print(f"Baseline: {baseline_result['score']:.4f}")

    # Iteration 1: Simple model
    simple_result = framework.simple_model()
    print(f"Simple model: {simple_result['score']:.4f}")

    # Iteration 2: Tree-based model
    tree_result = framework.tree_based_model()
    print(f"Tree model: {tree_result['score']:.4f}")

    # Analyze learning curve
    from sklearn.ensemble import RandomForestClassifier
    model = RandomForestClassifier(n_estimators=100, max_depth=10, random_state=42)
    lc_analysis = framework.analyze_learning_curve(model)
    print(f"\nLearning curve analysis: {lc_analysis['diagnosis']}")

    # Summary
    summary = framework.get_improvement_summary()
    print("\n=== Development Summary ===")
    print(summary)
```

### 4.3 Model Interpretability and Explainability

**Content:**
- Model-agnostic interpretation methods (SHAP, LIME)
- Feature importance analysis
- Partial dependence plots
- Individual prediction explanations
- Model debugging with interpretability

**Practical Example:**
- Explaining black-box model predictions
- Feature importance for business insights

**Code Listing 4.3: Model Interpretability**
```python
import shap
import lime
import lime.lime_tabular
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from typing import List, Dict

class ModelExplainer:
    """Comprehensive model interpretation tools"""

    def __init__(self, model, X_train, feature_names=None):
        self.model = model
        self.X_train = X_train
        self.feature_names = feature_names or [f"feature_{i}" for i in range(X_train.shape[1])]

    def shap_analysis(self, X_test, max_display=20):
        """SHAP (SHapley Additive exPlanations) analysis"""

        # Create SHAP explainer
        explainer = shap.TreeExplainer(self.model)
        shap_values = explainer.shap_values(X_test)

        # Summary plot
        plt.figure(figsize=(12, 8))
        shap.summary_plot(
            shap_values,
            X_test,
            feature_names=self.feature_names,
            max_display=max_display,
            show=False
        )
        plt.tight_layout()

        # Feature importance
        feature_importance = np.abs(shap_values).mean(axis=0)
        importance_df = pd.DataFrame({
            'feature': self.feature_names,
            'importance': feature_importance
        }).sort_values('importance', ascending=False)

        return {
            'shap_values': shap_values,
            'feature_importance': importance_df
        }

    def explain_instance_shap(self, instance, index=0):
        """Explain single prediction with SHAP"""

        explainer = shap.TreeExplainer(self.model)
        shap_values = explainer.shap_values(instance.reshape(1, -1))

        # Force plot
        shap.force_plot(
            explainer.expected_value,
            shap_values[0],
            instance,
            feature_names=self.feature_names,
            matplotlib=True
        )

    def lime_explanation(
        self,
        instance,
        num_features=10,
        num_samples=5000
    ):
        """LIME (Local Interpretable Model-agnostic Explanations)"""

        # Create LIME explainer
        explainer = lime.lime_tabular.LimeTabularExplainer(
            self.X_train,
            feature_names=self.feature_names,
            class_names=['Class 0', 'Class 1'],
            mode='classification'
        )

        # Explain instance
        exp = explainer.explain_instance(
            instance,
            self.model.predict_proba,
            num_features=num_features,
            num_samples=num_samples
        )

        # Get explanation
        explanation = exp.as_list()

        # Plot
        exp.as_pyplot_figure()
        plt.tight_layout()

        return {
            'explanation': explanation,
            'lime_object': exp
        }

    def permutation_importance(self, X_test, y_test, n_repeats=10):
        """Calculate permutation importance"""
        from sklearn.inspection import permutation_importance

        result = permutation_importance(
            self.model, X_test, y_test,
            n_repeats=n_repeats,
            random_state=42,
            n_jobs=-1
        )

        # Create dataframe
        importance_df = pd.DataFrame({
            'feature': self.feature_names,
            'importance_mean': result.importances_mean,
            'importance_std': result.importances_std
        }).sort_values('importance_mean', ascending=False)

        # Plot
        fig, ax = plt.subplots(figsize=(10, 8))
        importance_df.head(20).plot.barh(
            x='feature',
            y='importance_mean',
            xerr='importance_std',
            ax=ax
        )
        ax.set_xlabel('Permutation Importance')
        ax.set_title('Feature Importance (Permutation)')
        plt.tight_layout()

        return importance_df

    def partial_dependence_plot(
        self,
        features: List[int],
        grid_resolution=50
    ):
        """Partial dependence plots"""
        from sklearn.inspection import partial_dependence, PartialDependenceDisplay

        fig, ax = plt.subplots(figsize=(12, 4))
        display = PartialDependenceDisplay.from_estimator(
            self.model,
            self.X_train,
            features,
            feature_names=self.feature_names,
            grid_resolution=grid_resolution,
            ax=ax
        )

        plt.tight_layout()

        return display

    def generate_report(self, X_test, y_test) -> Dict:
        """Generate comprehensive interpretability report"""

        report = {
            'model_type': type(self.model).__name__,
            'n_features': len(self.feature_names)
        }

        # Feature importance (if available)
        if hasattr(self.model, 'feature_importances_'):
            report['feature_importance'] = pd.DataFrame({
                'feature': self.feature_names,
                'importance': self.model.feature_importances_
            }).sort_values('importance', ascending=False)

        # Permutation importance
        report['permutation_importance'] = self.permutation_importance(
            X_test, y_test
        )

        # SHAP values (if tree-based model)
        if hasattr(self.model, 'estimators_'):
            shap_results = self.shap_analysis(X_test[:100])  # Sample for speed
            report['shap_importance'] = shap_results['feature_importance']

        return report

# Usage
if __name__ == "__main__":
    from sklearn.ensemble import RandomForestClassifier
    from sklearn.datasets import make_classification
    from sklearn.model_selection import train_test_split

    # Generate data
    X, y = make_classification(
        n_samples=1000,
        n_features=20,
        n_informative=15,
        random_state=42
    )

    feature_names = [f"feature_{i}" for i in range(X.shape[1])]
    X_train, X_test, y_train, y_test = train_test_split(
        X, y, test_size=0.2, random_state=42
    )

    # Train model
    model = RandomForestClassifier(n_estimators=100, random_state=42)
    model.fit(X_train, y_train)

    # Create explainer
    explainer = ModelExplainer(model, X_train, feature_names)

    # Generate report
    report = explainer.generate_report(X_test, y_test)

    print("Top 10 most important features:")
    print(report['permutation_importance'].head(10))

    # Explain single instance
    instance = X_test[0]
    lime_exp = explainer.lime_explanation(instance)
    print("\nLIME explanation for instance 0:")
    print(lime_exp['explanation'])
```

---

## Chapter 5: Feature Engineering at Scale

### 5.1 Feature Engineering Fundamentals

**Content:**
- Feature types and transformations
- Automated feature engineering
- Domain-specific feature creation
- Temporal features
- Interaction features

**Practical Example:**
- E-commerce feature engineering
- Time-series feature creation

**Code Listing 5.1: Comprehensive Feature Engineering**
```python
import pandas as pd
import numpy as np
from datetime import datetime, timedelta
from typing import List, Dict, Callable
from sklearn.preprocessing import PolynomialFeatures

class FeatureEngineer:
    """Scalable feature engineering framework"""

    def __init__(self, config: Dict = None):
        self.config = config or {}
        self.feature_metadata = []

    def create_time_features(
        self,
        df: pd.DataFrame,
        datetime_col: str,
        features: List[str] = None
    ) -> pd.DataFrame:
        """Create time-based features"""

        if features is None:
            features = ['hour', 'day', 'month', 'year', 'dayofweek',
                       'quarter', 'is_weekend', 'is_month_start', 'is_month_end']

        df = df.copy()
        df[datetime_col] = pd.to_datetime(df[datetime_col])

        if 'hour' in features:
            df[f'{datetime_col}_hour'] = df[datetime_col].dt.hour

        if 'day' in features:
            df[f'{datetime_col}_day'] = df[datetime_col].dt.day

        if 'month' in features:
            df[f'{datetime_col}_month'] = df[datetime_col].dt.month

        if 'year' in features:
            df[f'{datetime_col}_year'] = df[datetime_col].dt.year

        if 'dayofweek' in features:
            df[f'{datetime_col}_dayofweek'] = df[datetime_col].dt.dayofweek

        if 'quarter' in features:
            df[f'{datetime_col}_quarter'] = df[datetime_col].dt.quarter

        if 'is_weekend' in features:
            df[f'{datetime_col}_is_weekend'] = (
                df[datetime_col].dt.dayofweek >= 5
            ).astype(int)

        if 'is_month_start' in features:
            df[f'{datetime_col}_is_month_start'] = (
                df[datetime_col].dt.is_month_start
            ).astype(int)

        if 'is_month_end' in features:
            df[f'{datetime_col}_is_month_end'] = (
                df[datetime_col].dt.is_month_end
            ).astype(int)

        # Cyclical encoding
        if 'hour_sin' in features:
            df[f'{datetime_col}_hour_sin'] = np.sin(
                2 * np.pi * df[datetime_col].dt.hour / 24
            )
            df[f'{datetime_col}_hour_cos'] = np.cos(
                2 * np.pi * df[datetime_col].dt.hour / 24
            )

        return df

    def create_aggregation_features(
        self,
        df: pd.DataFrame,
        group_cols: List[str],
        agg_col: str,
        agg_funcs: List[str] = None
    ) -> pd.DataFrame:
        """Create aggregation features"""

        if agg_funcs is None:
            agg_funcs = ['mean', 'std', 'min', 'max', 'count']

        df = df.copy()

        for func in agg_funcs:
            feature_name = f"{agg_col}_{func}_by_{'_'.join(group_cols)}"

            if func == 'count':
                agg = df.groupby(group_cols)[agg_col].transform('count')
            else:
                agg = df.groupby(group_cols)[agg_col].transform(func)

            df[feature_name] = agg

            self.feature_metadata.append({
                'feature_name': feature_name,
                'type': 'aggregation',
                'group_cols': group_cols,
                'agg_col': agg_col,
                'agg_func': func
            })

        return df

    def create_rolling_features(
        self,
        df: pd.DataFrame,
        datetime_col: str,
        value_col: str,
        windows: List[int] = None,
        agg_funcs: List[str] = None
    ) -> pd.DataFrame:
        """Create rolling window features"""

        if windows is None:
            windows = [7, 14, 30]

        if agg_funcs is None:
            agg_funcs = ['mean', 'std', 'min', 'max']

        df = df.copy()
        df = df.sort_values(datetime_col)

        for window in windows:
            for func in agg_funcs:
                feature_name = f"{value_col}_rolling_{window}d_{func}"

                if func == 'mean':
                    df[feature_name] = df[value_col].rolling(window).mean()
                elif func == 'std':
                    df[feature_name] = df[value_col].rolling(window).std()
                elif func == 'min':
                    df[feature_name] = df[value_col].rolling(window).min()
                elif func == 'max':
                    df[feature_name] = df[value_col].rolling(window).max()

                self.feature_metadata.append({
                    'feature_name': feature_name,
                    'type': 'rolling',
                    'window': window,
                    'agg_func': func
                })

        return df

    def create_interaction_features(
        self,
        df: pd.DataFrame,
        numeric_features: List[str],
        degree: int = 2
    ) -> pd.DataFrame:
        """Create polynomial interaction features"""

        poly = PolynomialFeatures(
            degree=degree,
            include_bias=False,
            interaction_only=True
        )

        X_poly = poly.fit_transform(df[numeric_features])

        # Get feature names
        feature_names = poly.get_feature_names_out(numeric_features)

        # Add to dataframe
        poly_df = pd.DataFrame(
            X_poly,
            columns=feature_names,
            index=df.index
        )

        # Drop original features (already in df)
        poly_df = poly_df.drop(columns=numeric_features)

        df = pd.concat([df, poly_df], axis=1)

        return df

    def create_lag_features(
        self,
        df: pd.DataFrame,
        value_col: str,
        lags: List[int] = None
    ) -> pd.DataFrame:
        """Create lag features"""

        if lags is None:
            lags = [1, 7, 14, 30]

        df = df.copy()

        for lag in lags:
            df[f'{value_col}_lag_{lag}'] = df[value_col].shift(lag)

        return df

    def create_categorical_features(
        self,
        df: pd.DataFrame,
        cat_col: str,
        encoding_type: str = 'frequency'
    ) -> pd.DataFrame:
        """Create categorical features"""

        df = df.copy()

        if encoding_type == 'frequency':
            freq = df[cat_col].value_counts(normalize=True)
            df[f'{cat_col}_frequency'] = df[cat_col].map(freq)

        elif encoding_type == 'target_mean':
            # Note: Should be fit only on training data
            target_mean = df.groupby(cat_col)['target'].mean()
            df[f'{cat_col}_target_mean'] = df[cat_col].map(target_mean)

        elif encoding_type == 'count':
            counts = df[cat_col].value_counts()
            df[f'{cat_col}_count'] = df[cat_col].map(counts)

        return df

    def create_text_features(
        self,
        df: pd.DataFrame,
        text_col: str
    ) -> pd.DataFrame:
        """Create text-based features"""

        df = df.copy()

        # Basic text features
        df[f'{text_col}_length'] = df[text_col].str.len()
        df[f'{text_col}_word_count'] = df[text_col].str.split().str.len()
        df[f'{text_col}_unique_words'] = df[text_col].apply(
            lambda x: len(set(str(x).split()))
        )
        df[f'{text_col}_avg_word_length'] = (
            df[f'{text_col}_length'] / df[f'{text_col}_word_count']
        )

        # Special characters
        df[f'{text_col}_num_uppercase'] = df[text_col].str.count(r'[A-Z]')
        df[f'{text_col}_num_digits'] = df[text_col].str.count(r'\d')
        df[f'{text_col}_num_special_chars'] = df[text_col].str.count(r'[^a-zA-Z0-9\s]')

        return df

    def create_all_features(
        self,
        df: pd.DataFrame,
        config: Dict
    ) -> pd.DataFrame:
        """Create all features based on configuration"""

        df = df.copy()

        # Time features
        if 'datetime_features' in config:
            for dt_config in config['datetime_features']:
                df = self.create_time_features(
                    df,
                    dt_config['column'],
                    dt_config.get('features')
                )

        # Aggregation features
        if 'aggregation_features' in config:
            for agg_config in config['aggregation_features']:
                df = self.create_aggregation_features(
                    df,
                    agg_config['group_cols'],
                    agg_config['agg_col'],
                    agg_config.get('agg_funcs')
                )

        # Rolling features
        if 'rolling_features' in config:
            for roll_config in config['rolling_features']:
                df = self.create_rolling_features(
                    df,
                    roll_config['datetime_col'],
                    roll_config['value_col'],
                    roll_config.get('windows'),
                    roll_config.get('agg_funcs')
                )

        # Lag features
        if 'lag_features' in config:
            for lag_config in config['lag_features']:
                df = self.create_lag_features(
                    df,
                    lag_config['value_col'],
                    lag_config.get('lags')
                )

        # Interaction features
        if 'interaction_features' in config:
            for int_config in config['interaction_features']:
                df = self.create_interaction_features(
                    df,
                    int_config['features'],
                    int_config.get('degree', 2)
                )

        return df

# Usage
if __name__ == "__main__":
    # Sample e-commerce data
    df = pd.DataFrame({
        'user_id': np.repeat(range(100), 10),
        'timestamp': pd.date_range('2024-01-01', periods=1000, freq='H'),
        'amount': np.random.exponential(50, 1000),
        'category': np.random.choice(['A', 'B', 'C'], 1000),
        'device': np.random.choice(['mobile', 'desktop'], 1000)
    })

    # Configuration
    config = {
        'datetime_features': [
            {
                'column': 'timestamp',
                'features': ['hour', 'dayofweek', 'is_weekend', 'hour_sin']
            }
        ],
        'aggregation_features': [
            {
                'group_cols': ['user_id'],
                'agg_col': 'amount',
                'agg_funcs': ['mean', 'std', 'count']
            }
        ],
        'rolling_features': [
            {
                'datetime_col': 'timestamp',
                'value_col': 'amount',
                'windows': [7, 30],
                'agg_funcs': ['mean', 'std']
            }
        ]
    }

    # Create features
    engineer = FeatureEngineer(config)
    df_features = engineer.create_all_features(df, config)

    print(f"Original features: {df.shape[1]}")
    print(f"After feature engineering: {df_features.shape[1]}")
    print(f"New features: {df_features.shape[1] - df.shape[1]}")
    print("\nSample features:")
    print(df_features.head())
```

Due to length constraints, I'll create a separate file for the remaining chapters (6-10). Let me continue...
