```
package com.example.mycityapp.navigation

import kotlinx.serialization.Serializable

@Serializable
sealed class NavigationRoute(val name: String) {
    data object StartScreen : NavigationRoute("Home")
    @Serializable
    data class Category(val categoryId: String) : NavigationRoute("category/{categoryId}")
    @Serializable
    data class Recommendation(val recId: String) : NavigationRoute("recommendation/{recId}")
}
```

```
import com.example.mycityapp.ui.CityViewModel  
import com.example.mycityapp.ui.components.RecommendationListAndDetails  
import com.example.mycityapp.ui.RecommendationScreen  
import com.example.mycityapp.ui.utils.CityContentType  
  
  
@Composable  
fun CityNavigation(  
    navController: NavHostController,  
    cityUiState: CityUiState,  
    viewModel: CityViewModel,  
    contentType: CityContentType,  
    modifier: Modifier = Modifier  
) {  
    NavHost(  
        navController = navController,  
        startDestination = NavigationRoute.StartScreen.name  
    ) {  
        composable(NavigationRoute.StartScreen.name) {  
            CityHomeScreen(  
                cityUiState = cityUiState,  
                contentType = contentType,  
                onCategoryClickedToUpdate = { category ->  
                    viewModel.updateCurrentCategory(category)  
                },  
                onRecommendationClickedToUpdate = { recommendation ->  
                    viewModel.updateCurrentRecommendation(recommendation)  
                },  
                onCategoryClickedToNavigate = { category ->  
                    navController.navigate(  
                        NavigationRoute.Category(category.id)  
                    )  
                    viewModel.updateCurrentCategory(category)  
                },  
                onRecommendationClickedToNavigate = { recommendation ->  
                    navController.navigate(  
                        NavigationRoute.Recommendation(recommendation.id)  
                    )  
                    viewModel.updateCurrentRecommendation(recommendation)  
                    viewModel.setLastToCurrentScreen()  
                },  
                modifier = modifier  
            )  
        }  
        composable<NavigationRoute.Category> { backstackEntry ->  
  
            CategoryScreen(  
                uiState = cityUiState,  
                onRecommendationClicked = { recommendation ->  
                    navController.navigate(  
                        route = NavigationRoute.Recommendation(recommendation.id)  
                    )  
                    viewModel.updateCurrentRecommendation(recommendation)  
                },  
                modifier = modifier  
            )  
        }  
        composable<NavigationRoute.Recommendation> { backstackEntry ->  
            if (contentType == CityContentType.LIST_DETAILS) {  
                RecommendationListAndDetails(  
                    cityUiState = cityUiState,  
                    onRecommendationClickedToUpdate = { recommendation ->  
                        viewModel.updateCurrentRecommendation(recommendation)  
                        viewModel.setLastToCurrentScreen()  
                    },  
                    modifier = modifier  
                )  
            } else {  
                RecommendationScreen(  
                    uiState = cityUiState,  
                    modifier = modifier  
                )  
            }  
        }  
    }}
```

Navigation uses now serializable objects to determine destinations